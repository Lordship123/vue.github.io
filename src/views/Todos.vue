<template>
  <div id="app">

<!-- Без перезагрузки переход -->
  <router-link to="/" class="home">Home</router-link>
    <AddItem
    @add-todo = "addTodo"
     />


     <select v-model="filter">
       
      <option value="all">All</option>
      <option value="completed">completed</option>
      <option value="not-completed">not-completed</option>

     </select>
     <hr>


   <Loader v-if="loading" />
    <TodoList 
    v-else-if="filteredTodos.length"
     v-bind:todos="filteredTodos"
    @remove-todo="removeTodo"
    />
  <p v-else>No todos!</p>
    
  </div>
</template>





<script>
import TodoList from '@/components/TodoList'
import AddItem from '@/components/AddItem'
import Loader from '@/components/Loader'


export default {
  name: 'App',
  data(){
    return {
      todos: [],
      loading: true,
      filter: 'all'
    }
  },

  components: {
    TodoList, AddItem, Loader
  },

  mounted(){
    fetch('https://jsonplaceholder.typicode.com/todos/?_limit=10')
  .then(response => response.json())

  .then(json => {
    setTimeout(() => {
    this.todos = json
    this.loading = false
  }, 1500)
  
})

  },

// watch: { // смотрит за измеиния

// filter(value){

// }


// },

computed: {
  filteredTodos () {
    if (this.filter === 'all'){
      return this.todos
    }

    if (this.filter === 'completed'){
      return this.todos.filter(t => t.completed)
    }

     if (this.filter === 'not-completed'){
      return this.todos.filter(t => !t.completed)
    }
    return null;

  }
},




  methods: {
    removeTodo(id){
      this.todos = this.todos.filter(t => t.id !== id)
    },
    addTodo(todo){
      this.todos.push(todo)
    }
  }
}
</script>





<style>

.home{

margin: 10px;

}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
