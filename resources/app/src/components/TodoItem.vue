<template>
    <li class="todo-item">
        <input type="checkbox" @change="updateComplete" v-model="item.completed" />

        <span :class="[item.completed ? 'todo-item--completed' : '', 'todo-item__itemText']">{{ item.name }}</span>

        <button class="todo-item__remove" @click="removeTodo">
            Remove Todo
        </button>
    </li>
</template>

<script>
export default {
    name: 'TodoItem',
    props: {
        item: Object,
    },
    methods: {
        updateComplete() {
            this.item.complete = !this.item.complete;

            fetch(`/api/item/${this.item.id}`, {
                method: 'PUT',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({
                    item: this.item,
                }),
            })
                .then(response => response.json())
                .then(json => {
                    console.log(json);
                    this.$emit('itemchanged');
                })
                .catch(error => console.log(error));
        },
        removeTodo() {
            fetch(`/api/item/${this.item.id}`, {
                method: 'DELETE',
                headers: {
                    'Content-Type': 'application/json',
                },
            })
                .then(response => response.json())
                .then(() => {
                    this.$emit('itemDeleted', this.item.id);
                })
                .catch(error => console.log(error));
        },
    },
};
</script>

<style lang="scss" scoped>
.todo-item {
    display: flex;

    &--completed {
        text-decoration: line-through;
        color: #999999;
    }

    &__itemText {
        width: 100%;
        margin: 0 0 0 0.5rem;
    }

    &__remove {
        white-space: nowrap;
        margin: 0.5rem 0;
    }
}
</style>
