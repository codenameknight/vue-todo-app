<script setup>
defineProps({
  filter: String,
  hasCompleted: Boolean,
});

const emit = defineEmits(["changeFilter", "clearCompleted"]);
</script>

<template>
  <div class="d-flex gap-2 flex-wrap">
    <div class="btn-group">
      <button
        class="btn btn-sm"
        :class="filter === 'all' ? 'btn-primary' : 'btn-outline-primary'"
        @click="emit('changeFilter', 'all')"
      >
        All
      </button>

      <button
        class="btn btn-sm"
        :class="filter === 'active' ? 'btn-primary' : 'btn-outline-primary'"
        @click="emit('changeFilter', 'active')"
      >
        Active
      </button>

      <button
        class="btn"
        :class="filter === 'completed' ? 'btn-primary' : 'btn-outline-primary'"
        @click="emit('changeFilter', 'completed')"
      >
        Completed
      </button>

      <button
        class="btn btn-sm btn-outline-danger ms-auto"
        @click="emit('clearCompleted')"
        :disabled="!hasCompleted"
      >
        Clear Completed
      </button>
    </div>
  </div>
</template>
