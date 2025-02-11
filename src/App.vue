<script setup>
import { ref, computed } from "vue";

const todos = ref([]);
const newTodo = ref("");
const hideCompleted = ref(false);

const filteredTodos = computed(() => {
  if (hideCompleted.value) {
    return todos.value.filter((todo) => !todo.completed);
  }
  return todos.value;
});

const addTodo = () => {
  if (newTodo.value.trim()) {
    todos.value.push({
      id: Date.now(),
      text: newTodo.value,
      completed: false,
    });
    newTodo.value = "";
  }
};

const removeTodo = (id) => {
  todos.value = todos.value.filter((todo) => todo.id !== id);
};

const toggleComplete = (id) => {
  const todo = todos.value.find((todo) => todo.id === id);
  if (todo) {
    todo.completed = !todo.completed;
  }
};
</script>

<template>
  <div class="todo-app">
    <h1>Todoリスト</h1>

    <div class="input-section">
      <input
        v-model="newTodo"
        @keyup.enter="addTodo"
        placeholder="新しいタスクを入力"
      />
      <button @click="addTodo">追加</button>
    </div>

    <button @click="hideCompleted = !hideCompleted">
      {{ hideCompleted ? "全て表示" : "完了済みを非表示" }}
    </button>

    <ul class="todo-list">
      <li
        v-for="todo in filteredTodos"
        :key="todo.id"
        :class="{ completed: todo.completed }"
      >
        <input
          type="checkbox"
          :checked="todo.completed"
          @change="toggleComplete(todo.id)"
        />
        <span>{{ todo.text }}</span>
        <button @click="removeTodo(todo.id)">✕</button>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.todo-app {
  max-width: 500px;
  margin: 0 auto;
  padding: 20px;
}

.input-section {
  margin-bottom: 20px;
}

button {
  margin-left: 10px;
}

.todo-list {
  margin-top: 20px;
}

li {
  padding: 10px;
  border-bottom: 1px solid #FFFFFF;
}

.completed {
  text-decoration: line-through;
  color: #888;
}
</style>
