<template>
    <ul class="todo-list">
        <TodoItem :key="index" v-on="$listeners" v-for="(item, index) in items" :item="item" />
    </ul>
</template>

<script>
import TodoItem from './TodoItem';

export default {
    name: 'TodoList',
    props: {
        items: Array,
    },
    components: {
        TodoItem,
    },
};
</script>

<style lang="scss" scoped>
.todo-list {
    list-style-type: none;
    padding: 0;
}
</style>
