<template>
    <div class="home">
        <AddTodo @itemAdded="itemAdded" />

        <TodoList :items="items" @itemDeleted="itemDeleted" />
    </div>
</template>

<script>
import AddTodo from '@/components/AddTodo';
import TodoList from '@/components/TodoList';

export default {
    name: 'Home',
    components: { AddTodo, TodoList },
    data() {
        return {
            items: [],
        };
    },
    methods: {
        getTodos() {
            fetch(`/api/items`)
                .then(response => response.json())
                .then(response => {
                    this.items = response;
                })
                .catch(error => console.log(error));
        },
        itemDeleted(itemId) {
            this.items = this.items.filter(item => item.id !== itemId);
        },
        itemAdded(item) {
            this.items = [item, ...this.items];
        },
    },
    mounted() {
        this.getTodos();
    },
};
</script>
