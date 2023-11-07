<template>
    <div v-for="(t,index) in todos" :key="t.id" class="card mt-2">
      <div class="card-body p-2 d-flex align-items-">
        <div class="form-check flex-grow-1">
          <input class="form-check-input" type="checkbox" :checked="t.completed" @change="toggleTodo(index)" >
          <label class="form-check-label" :class="{todo: t.completed}" >{{t.subject}}</label>

          
        </div>
        <div>
          <button @click="deleteTodo(index)" class="btn btn-danger btn-sm">삭제</button>
        </div>
      </div>
    </div>
</template>

<script>
export default {
    //props를 이용하여 부모에게서 받아옴
    props: {
        todos: {
            type: Array,
            required: true
        }
    },
    emits: ['toggle-todo','delete-todo'],
    setup(props,{emit}) {
        const toggleTodo = (index) => {
            emit('toggle-todo', index);
        }

        const deleteTodo = (index) => {
            emit('delete-todo',index);
        };

        return {
            toggleTodo,
            deleteTodo
        }
    }
    

}
</script>

<style>

</style>
