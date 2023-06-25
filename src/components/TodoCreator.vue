<script setup>
import { ref, defineEmits } from 'vue';
import TodoButton from './TodoButton.vue';

const emit = defineEmits(["create-todo"]);

const todoState = ref({
    description: "",
    invalid: false,
    errorMsg: "",
});

const createTodo = () => {
    todoState.value.invalid = false;
    if (todoState.value.description) {
        emit("create-todo", todoState.value.description);
        todoState.value.description = "";
        return;
    }
    todoState.value.invalid = true;
    todoState.value.errorMsg = "Todo value cannot be empty";
}
</script>

<template>
    <div class="input-wrap" :class="{'input-err': todoState.invalid}">
        <input type="text" v-model="todoState.description"/>
        <TodoButton @click="createTodo()" />
    </div>
    <p v-show="todoState.invalid" class="err-msg">{{ todoState.errorMsg }}</p>
</template> 

<style lang="scss" scoped>
.input-wrap {
    display: flex;
    border: 2px solid #41b080;
    &.input-err {
        border-color: red;
    }
    input {
        width: 100%;
        padding: 8px 6px;
        border:  none;
        &:focus {
            outline: none;
        }
    }
}

.err-msg {
    margin-top: 6px;
    font-size: 12px;
    text-align: center;
    color: red;
}
</style>