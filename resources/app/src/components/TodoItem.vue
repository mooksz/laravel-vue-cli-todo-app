<template>
    <li class="todo-item">
        <input type="checkbox" @change="updateComplete" v-model="item.completed" />

        <span
            v-if="!changingName"
            @click="changeName"
            :class="[item.completed ? 'todo-item--completed' : '', 'todo-item__item-text']"
            >{{ item.name }}</span
        >

        <input
            type="text"
            class="todo-item__change-name-input"
            v-if="changingName"
            @keyup.enter="changeName(), updateName()"
            @blur="changeName(), updateName()"
            v-model="item.name"
        />

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
    data() {
        return {
            changingName: false,
        };
    },
    methods: {
        changeName() {
            this.changingName = !this.changingName;
        },
        updateName() {
            fetch(`/api/item/${this.item.id}`, {
                method: 'PUT',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({
                    item: this.item,
                }),
            }).catch(error => console.log(error));
        },
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
            }).catch(error => console.log(error));
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
    align-items: center;

    &--completed {
        text-decoration: line-through;
        color: #999999;
    }

    &__item-text {
        border: 2px solid white;
        padding: 1px 2px;
    }

    &__item-text,
    &__change-name-input {
        width: 100%;
        margin: 0.5rem;
    }

    &__remove {
        white-space: nowrap;
        margin: 0.5rem 0;
    }

    &__change-name-input {
        color: #2c3e50;
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        font-size: 1rem;
    }
}
</style>
