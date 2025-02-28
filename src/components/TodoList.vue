<template>
    <div>
        <input type="text" class="todo-input" v-model="newTodo" @keyup.enter="addTodo"
               placeholder="What needs to be done!">
        <div v-for="(todo, index) in todosFiltered" :key="todo.id" class="todo-item">
            <div class="todo-item-left">
                <input type="checkbox" v-model="todo.completed">
                <div v-if="!todo.editing" @dblclick="editTodo(todo)" class="todo-item-label"
                     :class="{ completed : todo.completed }"> {{ todo.title }}
                </div>
                <input v-else type="text" class="todo-item-edit" v-model="todo.title" @blur="doneEdit(todo)"
                       @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" placeholder="edit todo" v-focus>
            </div>
            <div class="remove-item" @click="removeTodo(index)">
                &times;
            </div>
        </div>

        <div class="extra-container">
            <div>
                <label><input type="checkbox" :checked="!anyRemaining" @change="checkAllTodos"> Check All</label>
            </div>
            <div>{{ remaining }} items left</div>
        </div>

        <div class="extra-container">
            <div>
                <button :class="{ active : filter == 'all' }" @click="filter = 'all'">All</button>
                <button :class="{ active : filter == 'active' }" @click="filter = 'active'">Active</button>
                <button :class="{ active : filter == 'completed' }" @click="filter = 'completed'">Completed</button>
            </div>
            <div>
                <transition name="fade">
                    <button v-if="showClearCompletedButton" @click="clearCompleted">Clear Completed</button>
                </transition>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'todo-list',
        data() {
            return {
                newTodo: '',
                idForTodo: 4,
                beforeEditCache: '',
                filter: 'all',
                todos: [
                    {
                        'id': 1,
                        'title': 'Finish vue todo app tutorial',
                        'completed': true,
                        'editing': false,
                    },
                    {
                        'id': 2,
                        'title': 'Implement pdf export on viewData project',
                        'completed': true,
                        'editing': false,
                    },
                    {
                        'id': 3,
                        'title': "Send an email to my supervisor reporting today's accomplishments",
                        'completed': true,
                        'editing': false,
                    },
                ]
            }
        },
        computed: {
            remaining() {
                return this.todos.filter(x => !x.completed).length
            },
            anyRemaining() {
                return this.remaining != 0
            },
            todosFiltered() {
                if (this.filter == 'all')
                    return this.todos
                else if (this.filter == 'active')
                    return this.todos.filter(x => !x.completed)
                else if (this.filter == 'completed')
                    return this.todos.filter(x => x.completed)
                return this.todos
            },
            showClearCompletedButton() {
                return this.todos.filter(todo => todo.completed).length > 0
            }
        },
        directives: {
            focus: {
                inserted: function (el) {
                    el.focus()
                }
            }
        },
        methods: {
            addTodo() {
                if (this.newTodo.trim().length == 0) {
                    return
                }

                this.todos.push({
                    'id': this.idForTodo,
                    'title': this.newTodo,
                    'completed': false
                });
                this.newTodo = ''
                this.idForTodo++
            },
            editTodo(todo) {
                this.beforeEditCache = todo.title
                todo.editing = true
            },
            doneEdit(todo) {
                if (todo.title.trim() == '') {
                    todo.title = this.beforeEditCache
                }
                todo.editing = false
            },
            cancelEdit(todo) {
                todo.title = this.beforeEditCache
                todo.editing = false
            },
            removeTodo(index) {
                this.todos.splice(index, 1)
            },
            checkAllTodos() {
                this.todos.forEach(todo => todo.completed = event.target.checked)
            },
            clearCompleted() {
                this.todos = this.todos.filter(x => !x.completed)
            }
        }
    }
</script>

<style>
    .todo-input {
        width: 100%;
        padding: 10px 18px;
        font-size: 18px;
        margin-bottom: 16px;
    }

    .todo-input:focus {
        outline: 3px;
    }

    .todo-item {
        margin-bottom: 12px;
        display: flex;
        align-items: center;
        justify-content: space-between;
    }

    .remove-item {
        cursor: pointer;
        margin-left: 14px;
    }

    .remove-item:hover {
        color: black;
    }

    .todo-item-left {
        display: flex;
        align-items: center;
    }

    .todo-item-label {
        padding: 10px;
        border: 1px solid white;
        margin-left: 12px;
    }

    .todo-item-edit {
        font-size: 24px;
        color: #2c3e50;
        margin-left: 12px;
        width: 100%;
        padding: 10px;
        border: 1px solid #ccc;
        font-family: 'Avenir', Helvetica, Arial, SansSerif;
    }

    .todo-item-edit:focus {
        outline: none;
    }

    .todo-item-label.completed {
        text-decoration: line-through;
        color: grey;
    }

    .extra-container {
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 16px;
        border-top: 1px solid lightgrey;
        padding-top: 14px;
        margin-bottom: 14px;
    }

    button {
        font-size: 14px;
        background-color: white;
        appearance: none;
        border: 1px solid #ccc;
        margin-left: 5px;
    }

    button:hover {
        background-color: lightgreen;
    }

    button:focus {
        outline: none;
    }

    .active {
        background-color: lightgreen;
    }

    .fade-enter-active, .fade-live-active {
        transition: opacity 2s;
    }

    .fade-enter, .fade-leave-to {
        opacity: 0;
    }
</style>