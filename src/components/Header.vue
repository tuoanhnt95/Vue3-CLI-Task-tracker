<template>
    <header>
        <h1>{{ title }}</h1>
        <Button 
            v-show="homePage"
            :text="computedText" 
            :color="computedColor" 
            @btn-click="$emit('toggle-add-task')"
        />
    </header>
</template>

<script>
import Button from './Button.vue'

export default {
    name: 'Header',
    props: {
        title: String,
        showAddTask: Boolean,
    },
    components: {
        Button
    },
    computed: {
        computedText() {
            return this.showAddTask ? 'Close' : 'Add Task'
        },
        computedColor() {
            return this.showAddTask ? 'red' : 'green'
        },
        homePage() {
            return this.$route.path === '/'
        }
    }
}
</script>

<style scoped>
    header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 20px;
    }
</style>