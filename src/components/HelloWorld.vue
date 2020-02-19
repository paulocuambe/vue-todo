<template>
    <div>
        <p> {{ hello }}</p>
        <p> {{ pessoa.eyes }}</p>

    </div>
</template>

<script>
    export default {
        props: ["msg"],
        data() {
            return {
                pessoa: {
                    eyes: "blue",
                    age: 21
                },
                hello: 'ola'
            }
        },
    }
</script>