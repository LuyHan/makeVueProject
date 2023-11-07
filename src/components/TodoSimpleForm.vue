<template>
    <form @submit.prevent="onSubmit" >
      <div class="d-flex">
        <div class="flex-grow-1 mr-2">
          <input type="text" class="form-control mr-4" v-model="todo" placeholder="일정을 입력하세요" />
        </div>
        <div>
          <button 
            class="btn btn-primary mr-2"
            type="submit"
          >추가</button>
        </div>
      </div>
      <div v-show="hasError" style="color:red;">
        내용을 입력해주세요
      </div>
    </form>
</template>

<script>
import {ref} from 'vue';
export default {
    emits: ['add-todo'],
    setup(props, {emit}) {
        const todo = ref('');
        const hasError = ref(false);

        const onSubmit = () =>  {
            if (todo.value === '') {
                hasError.value  = true;
            } else {
                //context의 emit을 통하여 부모에게 이벤트 전송
                emit('add-todo',{
                    id: Date.now(),
                    subject: todo.value,
                    completed: false,
                });
                hasError.value = false;

                todo.value = '';
            }
            
        };

        return {
            todo,
            hasError,
            onSubmit,
        }
    }
}
</script>

<style>

</style>