<template>
    <div>
        <input type="text" class="todo-input" v-model="newTodo" @keyup.enter="addTodo"
               placeholder="What needs to be done!">
        <div v-for="todo in todos" :key="todo.id" class="todo-item">
            <div>
                {{ todo.title }}
            </div>
            <div class="remove-item">
                &times;
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
                todos: [
                    {
                        'id': 1,
                        'title': 'Finish vue todo app tutorial',
                        'completed': false
                    },
                    {
                        'id': 2,
                        'title': 'Implement pdf export on viewData project',
                        'completed': false
                    },
                    {
                        'id': 3,
                        'title': "Send an email to my supervisor reporting today's accomplishments",
                        'completed': false
                    },
                ]
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
</style>