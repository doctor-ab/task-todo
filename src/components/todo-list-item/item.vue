<template>
    <li class="list-item todo__list-item">
        
        <span v-bind:class="{done: todo.completed}" class="list-item__title">
            
            <input :checked="completed"
                   type="checkbox" 
                   class="list-item__checkbox" 
                   v-on:change="$emit('is-completed', index)"
            />

            <strong class="list-item__id">{{ index+1 }}</strong>
            
            <span class="list-item__span" contenteditable @input="event => onInput(event, index)">{{ todo.title }}</span>
            
        </span>
        <div class="list-item__control">  
            <button class="list-item__delete"
                    v-on:click="$emit('remove-todo', todo.id)"
            ></button>

            <button class="list-item__edit"
                    v-on:click="editTodo(index)"
                    ></button>
        </div>  
    </li>
</template>


<script>

    export default{
         data(){
            return {
                title: '',
                element: '',
            }
        },
        mounted(){
          
        },
        props: {
            todo: {
                type: Object,
                required: true
            },
            index: Number,
            completed: Boolean
        },
        methods: {
            onInput(event) {
                this.element = event;
                this.title = event.target.innerText;
        
            },
            editTodo(id){
                if(this.title.length > 0){
                    this.element.target.innerText=this.title;
                    this.$emit('edit-todo', id, this.title)
                }
            }
        }
    }
</script>

<style scoped>
    .done {
        text-decoration: line-through;
    }
</style>