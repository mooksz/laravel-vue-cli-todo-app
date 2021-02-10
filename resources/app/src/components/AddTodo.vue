<template>
    <div class="add-todo">
        <h2 class="add-todo__title">Add todo</h2>

        <div class="add-todo__input-wrapper">
            <input type="text" v-model="item.name" @keyup.enter="addTodo" />
            <button @click="addTodo" :disabled="!item.name">Add</button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'AddTodo',
    data() {
        return {
            item: {
                name: '',
            },
        };
    },
    methods: {
        addTodo() {
            if (!this.item.name) return;

            fetch(`api/item/store`, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({ item: this.item }),
            })
                .then(response => response.json())
                .then(json => {
                    console.log(json);
                    this.$emit('itemAdded', {
                        name: this.item.name,
                        completed: false,
                    });
                    this.item.name = '';
                })
                .catch(error => console.log(error));
        },
    },
};
</script>

<style lang="scss" scoped>
.add-todo {
    &__title {
        margin: 0;
    }

    &__input-wrapper {
        display: flex;
        align-items: center;
    }
}
</style>
