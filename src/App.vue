<template>
  <div id="AppContainer">
    <h1>Todo List</h1>
    <form @submit.prevent="addTodos()">
        <input type="text" v-model="newTodo">
        <button class="waves-effect waves-light btn">Add Todos</button>
    </form>
    <div class="todoList">
      <div v-for="(todo, index) in todos"
       :key="index" :class="{ completed: todo.complete }"
       @click="completedTodo(todo)"
       >
        <div class="row">
          <div class="col s12 m6">
              <div class="card teal teal lighten-1">
                  <div class="card-content white-text">
                    <span>{{ todo.text }}</span>
                  </div>
              </div>
          </div>
        </div>
      </div>
    </div>
    <button class="wave-effect wave-light red lighten-2 mb-3 btn"
    v-if="todos.length !== 0"
    @click="removeTodo"
    >remove all todos</button>
  </div>
</template>

<script>
import { ref } from 'vue';
export default {
  name: 'App',
  setup(){
    let newTodo = ref('');
    let initialLoadData = [
      {
        completed: false,
        text: "create todo list functionality"
      }
    ];
    let storedTodos;
    localStorage.getItem('todos')
    ? (storedTodos = JSON.parse(localStorage.getItem('todos')))
    : (storedTodos = initialLoadData);
    
    const todos = ref(storedTodos);

    function addTodos(){
      if(newTodo.value !== "" ) {
        todos.value.push({
          completed: false,
          text: newTodo.value
        });
        newTodo.value = "";
        localStorage.setItem('todos', JSON.stringify(todos.value));  // Save the new todo list in localStorage
      }
    }

    function removeTodo(){
       todos.value.pop()
    }
    function completedTodo(todo){
      todo.complete = !todo.complete
    }

    return { addTodos, todos, newTodo, removeTodo, completedTodo }
  }
}
</script>

<style scoped>
.completed {
  text-decoration: line-through;
}
#AppContainer{
  padding: 2%;
}
</style>
