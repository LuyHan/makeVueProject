<template>

  <div class="container">

    <h2>To-Do List</h2>
    <input type="text" class="form-control" v-model="searchText" placeholder="검색" />
    <hr>
    <TodoSimpleForm @add-todo="addTodo" />
    <div style="color:red;">{{ error }}</div>

    <div v-if="!filteredTodos.length">
      추가된 일정이 없습니다.
    </div>
    
    <TodoList :todos="filteredTodos" @toggle-todo="toggleTodo" @delete-todo="deleteTodo" />

  </div>
 </template>

<script>
import {ref,computed} from 'vue' 
import TodoSimpleForm from './components/TodoSimpleForm.vue'
import TodoList from './components/TodoList.vue'
import axios from 'axios';
export default {
  components: {
    TodoSimpleForm,
    TodoList
  },  
  setup() {

    const todos = ref([]);
    const error = ref('');

    const getTodos = async () => {
      try {
        const res = await axios.get('http://localhost:3000/todos');
        todos.value = res.data;
      } catch(err) {
        console.log(err);
        error.value = '문제가 발생하였습니다.'
      }
      
    }

    getTodos();
    
    const addTodo = async (todo) => {
      error.value = '';
      // 데이터베이스 todo 저장 post http save
      try {
        const res = await axios.post('http://localhost:3000/todos',{
          subject: todo.subject,
          completed: todo.completed,
        });
        todos.value.push(res.data)
      } catch(err) {
        error.value = '문제가 발생하였습니다.'
        console.log(err);
      }
      
    };
    

    const deleteTodo = async (index) => {
      error.value = '';
      const id = todos.value[index].id;
      try {
        await axios.delete('http://localhost:3000/todos/'+id);
        getTodos();
      } catch(err) {
        error.value = '문제가 발생하였습니다.'
        console.log(err);
      }
      
    };

    const toggleTodo = async (index) => {
      error.value = '';
      const id = todos.value[index].id;
      try {
        await axios.patch('http://localhost:3000/todos/'+id, {
          completed: !todos.value[index].completed
        });
        todos.value[index].completed = !todos.value[index].completed;
      } catch(err) {
        error.value = '문제가 발생하였습니다.'
        console.log(err);
      }
      
    };

    const searchText = ref('');
    const filteredTodos = computed(()=>{
      if (searchText.value) {
        return todos.value.filter(todo => {
          return todo.subject.includes(searchText.value);
        });
      }

      return todos.value
    })

    return {
      todos,
      deleteTodo,
      addTodo,
      toggleTodo,
      searchText,
      filteredTodos,
      error,
    }
  }
}
</script>

<style>
  .todo {
    color:gray;
    text-decoration: line-through;
  }
</style>
