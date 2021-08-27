<template>
  <form class="todo__form" @submit.prevent="onSubmit()">
      
    <input type="text" 
           class="todo__input"
           v-model="title"
           placeholder="Enter New To-Do"
           @input="$emit('on-filter', title)"
           />
    <button type="submit" class="todo__btn">&rarr;</button>
  </form>
</template>

<script>
    import {v4 as uuidv4} from "uuid"

    export default {
        data(){
            return {
                title: ""
            }
        },
        methods: {
            onSubmit() {
                if(this.title.trim()){
                    const newTodo = {
                        id: uuidv4(),
                        title: this.title,
                        completed: false,
                    }

                    this.$emit('add-todo', newTodo);

                    this.title = "";
                }
            }
        }
    }
</script>