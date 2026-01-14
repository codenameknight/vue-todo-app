<script setup>
defineProps({
  todo: Object,
});

const emit = defineEmits(["remove"]);
</script>

<template>
  <li class="list-group-item d-flex align-items-center justify-content-between">
    <div class="form-check">
      <input
        class="form-check-input"
        type="checkbox"
        v-model="todo.done"
        :id="`todo-${todo.id}`"
      />
      <label
        class="form-check-label"
        :class="{ 'text-decoration-line-through text-muted': todo.done }"
        :for="`todo-${todo.id}`"
      >
        {{ todo.text }}
      </label>
    </div>

    <button
      class="btn btn-sm btn-outline-danger"
      @click="emit('remove', todo.id)"
    >
      Delete
    </button>
  </li>
</template>
