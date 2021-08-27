<template>
    <div class="todo__list">
            <ul class="todo__list-group">
                <TodoItem 
                    v-for="(todo, i) of todos"
                    :key="todo.i"
                    v-bind:todo="todo"
                    v-bind:index="i"
                    v-bind:completed="todo.completed"
                    v-on:remove-todo="removeTodo"
                    v-on:edit-todo="editTodo"
                    v-on:is-completed="isCompleted"
                />
            </ul>
        </div>
</template>

<script>
    import TodoItem from '../todo-list-item/item.vue';


    export default {
        props: ['todos'],
        components: {
            TodoItem
        },
        methods: {
            removeTodo(id){
                this.$emit('remove-todo', id)
            },
            editTodo(id, title){
                this.$emit('edit-todo', id, title)
            },
            isCompleted(id){
                this.$emit('is-completed', id)
            }
        }
    }

</script>