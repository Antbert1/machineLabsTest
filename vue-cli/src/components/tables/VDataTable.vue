<template>
    <div>
        <p>{{ test }}</p>
        <button @click="changeText">Change</button>
    </div>
</template>

<script>
export default {
    data: function() {
        return {
            test: "test"
        }
    },
    methods: {
        changeText() {
            this.test = "test2"
        }
    }
}
</script>