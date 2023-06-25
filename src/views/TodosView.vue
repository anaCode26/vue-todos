<script setup>
import { ref , watch, computed} from 'vue';
import { uid } from 'uid';
import { Icon } from "@iconify/vue"
import TodoCreator from '../components/TodoCreator.vue';
import TodoItem from '../components/TodoItem.vue';

const todos = ref([]);

watch(
  todos,
  () => {
    setTodosLocalStorage();
  },
  {
    deep: true,
  }
)

const todoCompleted = computed(() => {
  return todos.value.every((todo) => todo.isCompleted);
})

const fetchTodos = () => {
  const savedTodos = JSON.parse(localStorage.getItem("todos"))
  if (savedTodos) {
    todos.value = savedTodos;
  }
}

fetchTodos();

const setTodosLocalStorage = () => {
  localStorage.setItem("todos",JSON.stringify(todos.value) )
}

const createTodo = (description) => {
  todos.value.push({
    id: uid(),
    description,
    isCompleted: false,
    isEditing: false,
  })
  setTodosLocalStorage();
};

const toggleTodoComplete = (index) => {
  todos.value[index].isCompleted = !todos.value[index].isCompleted;
}

const editTodo = (index) => {
  todos.value[index].isEditing = !todos.value[index].isEditing;
  setTodosLocalStorage();
}

const updateTodo = (todoVal, index) => {
  todos.value[index].description = todoVal; 
  setTodosLocalStorage();
}

const deleteTodo = (todoId) => {
  todos.value = todos.value.filter((todo) => todo.id != todoId);
  setTodosLocalStorage();
};

</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <TodoCreator @create-todo="createTodo"/>
    <ul class="todo-list" v-if="todos.length > 0">
      <TodoItem v-for="(todo, index) in todos" :todo="todo" :index="index" @toggle-complete="toggleTodoComplete" @click-edit="editTodo" @click-save="updateTodo" @click-delete="deleteTodo" />
    </ul> 
    <p class="todos-msg" v-else>
      <Icon icon="noto-v1:sad-but-relieved-face" width="22"/>
      <span>You have no todo's to complete! Add one!</span>
    </p> 
    <p v-if="todoCompleted && todos.length > 0" class="todos-msg">
      <Icon icon="noto-v1:party-popper" width="22"/>
      <span>
        You have completed all your todos!</span>
    </p>  
  </main>
</template>

<style lang="scss" scoped>
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 20px;
    h1 {
      margin-bottom: 20px;
      text-align: center;
    }
    .todo-list {
    display: flex;
    flex-direction: column;
    list-style: none;
    margin-top: 24px;
    gap: 20px;
  }

  .todos-msg {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-top: 24px;
  }
}
</style>
