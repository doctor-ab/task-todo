<template>
  <div class="todo">
      <div class="_container">
        <h3 class="todo__title" contenteditable @input="event => onTitle(event)">{{titleApp}}</h3>
        
        <div class="todo__body">
            <div class="todo__status">
                <div class="status status_total"> Total: {{todos.length}}</div>
                <div class="status status_success"> Succes: {{completedCount}}</div>
                <div class="status status_deleted">Deleted: {{deleteCount}}</div>
            </div>
    
            <InputTodo 
                v-on:add-todo="addTodo"
                v-on:on-filter="onFilter"
            />
            <select class="todo__select" v-model="filter" v-on:change="onChangeFilter($event);">
                <option value="all">All</option>
                <option value="completed">Completed</option>    
                <option value="not-completed">Not Completed</option>    
            </select>    
            <TodoList
                v-bind:todos="filteredTodos"
                v-on:remove-todo="removeTodo"
                v-on:edit-todo="editTodo"
                v-on:is-completed="isCompleted"
            />
        </div>
    </div>
  </div>
</template>

<script>
    import TodoList from '../todo-list/list.vue'
    import InputTodo from '../todo-input/input.vue'

    export default {
        

        name: 'App',
        data(){
            return {
                todos: [
                    {id:1, title: "Создать to do", completed: false},
                    {id:2, title: "Стилизовать через scss", completed: false},
                    {id:3, title: "Запушить", completed: false},
                ],
                filter: 'all',
                search:'',
                deleteCount:0,
                completedCount:0,
                titleApp: 'To-Do List',
            }
        },
        mounted(){
            
            document.title = localStorage.titleApp || this.titleApp;
            this.titleApp = document.title
            this.todos = JSON.parse(localStorage.getItem("todo-storage") || '[]');
            this.filter = localStorage.filter;
            
            if(localStorage.deleteCount){
                this.deleteCount =  Number(localStorage.deleteCount);
            }
            this.checkComplited();
        },
        computed: {
            filteredTodos(){
                if(this.search.length > 0){
                     return this.todos.filter((todo)=>{
                        return todo.title.match(this.search)
                    })
                }

                if(this.filter === "all"){
                    return this.todos
                }
                if(this.filter === "completed"){
                    return this.todos.filter(t => t.completed)
                }
                if(this.filter === "not-completed"){
                    return this.todos.filter(t => !t.completed)
                }
                return this.todos
            },


        },
        methods:{
            onTitle(event){
                document.title = event.target.innerText;
                localStorage.titleApp = event.target.innerText;
            },
            onChangeFilter(event){
                localStorage.filter = event.target.value; 
            },
            removeTodo(id){
                this.todos = this.todos.filter(t => t.id !== id);
                this.setLocalToDo();
                this.deleteCount += 1;
                localStorage.deleteCount = this.deleteCount;
            },
            editTodo(id, title){
                this.todos[id].title = title;
                this.setLocalToDo();
            },
            addTodo(todo){
                this.todos.push(todo);
                this.setLocalToDo();
                this.search = '';
            },
            isCompleted(id){
                this.todos[id].completed = !this.todos[id].completed;
                this.setLocalToDo();
                this.checkComplited();
            },
            onFilter(text){
                this.search = text;
            },
            setLocalToDo(){
                localStorage.setItem("todo-storage", JSON.stringify(this.todos));
            },
            checkComplited(){
                this.completedCount = 0;
                for(let i of this.todos){
                    if(i.completed){
                        this.completedCount += 1;
                    }
                }
            }
            

        
        },
        components: {
            TodoList,
            InputTodo
        }
    }
</script>
