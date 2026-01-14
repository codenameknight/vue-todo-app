<script setup>
import { computed, onMounted, ref, watch } from "vue";
import Filters from "./components/Filters.vue";
import TodoItem from "./components/TodoItem.vue";

const newTodo = ref("");
const todos = ref([]);
const filter = ref("all"); // all | active | completed
const theme = ref("light"); // light | dark theme

// LocalStorage'dan Okuma
onMounted(() => {
  // default theme
  document.documentElement.setAttribute("data-bs-theme", theme.value);

  const savedTodos = localStorage.getItem("todos");
  if (savedTodos) {
    todos.value = JSON.parse(savedTodos);
  }
});

// LocalStorage'a Kaydetme
watch(
  todos,
  (newTodos) => {
    localStorage.setItem("todos", JSON.stringify(newTodos));
  },
  { deep: true }
);

const hasCompleted = computed(() => {
  return todos.value.some((todo) => todo.done);
});

const remainingCount = computed(() => {
  return todos.value.filter((todo) => !todo.done).length;
});

// Filtrelenmiş liste
const filteredTodos = computed(() => {
  if (filter.value === "active") {
    return todos.value.filter((todo) => !todo.done);
  }

  if (filter.value === "completed") {
    return todos.value.filter((todo) => todo.done);
  }

  return todos.value; // all
});

function addTodo() {
  if (newTodo.value.trim() === "") return;

  todos.value.push({
    id: Date.now(),
    text: newTodo.value,
    done: false,
  });

  newTodo.value = "";
}

function removeTodo(id) {
  todos.value = todos.value.filter((todo) => todo.id !== id);
}

function clearCompleted() {
  todos.value = todos.value.filter((todo) => !todo.done);
}

function toggleTheme() {
  theme.value = theme.value === "light" ? "dark" : "light";

  document.documentElement.setAttribute("data-bs-theme", theme.value);
}
</script>

<template>
  <div class="container mt-5">
    <div class="card shadow-sm">
      <div class="card-body">
        <div class="d-flex justify-content-between align-items-center">
          <button class="btn btn-sm btn-outline-secondary" @click="toggleTheme">
            {{ theme === "light" ? "🌙 Dark" : "☀️ Light" }}
          </button>
        </div>

        <h1 class="card-title mb-4 text-center">To-Do List</h1>

        <!-- Todo ekleme -->
        <div class="input-group mb-3">
          <input
            v-model="newTodo"
            class="form-control"
            placeholder="
Add new todo"
            @keyup.enter="addTodo"
          />
          <button class="btn btn-primary" @click="addTodo">Add</button>
        </div>

        <!-- Filters -->
        <Filters
          :filter="filter"
          :hasCompleted="hasCompleted"
          @changeFilter="filter = $event"
          @clearCompleted="clearCompleted"
        ></Filters>

        <p class="text-muted mt-3">{{ remainingCount }} items left</p>

        <!-- Todo list -->
        <ul class="list-group">
          <TodoItem
            v-for="todo in filteredTodos"
            :key="todo.id"
            :todo="todo"
            @remove="removeTodo"
          />
        </ul>
      </div>
    </div>
  </div>
</template>
