<template>
  <div class="card mb-4 shadow-sm">
    <div class="card-body">
      <div class="mb-3">
        <input
          v-model="searchQuery"
          type="text"
          class="form-control"
          placeholder="🔍 Поиск по названию или автору..."
        />
      </div>

      <div class="btn-group mb-3 w-100" role="group">
        <button
          v-for="option in filterOptions"
          :key="option.value"
          @click="$emit('update:filter', option.value)"
          :class="[
            'btn',
            filter === option.value ? 'btn-primary' : 'btn-outline-primary',
          ]"
        >
          {{ option.label }}
        </button>
      </div>

      <div class="alert alert-info py-2 mb-0">
        <small>
          Всего: <span class="badge bg-primary">{{ total }}</span> | Прочитано:
          <span class="badge bg-success">{{ completed }}</span> | Осталось:
          <span class="badge bg-warning">{{ total - completed }}</span> |
          Избранно: <span class="badge bg-danger">{{ favorite }}</span>
        </small>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from "vue";

const props = defineProps(["filter", "books"]);
defineEmits(["update:filter"]);
const searchQuery = defineModel("searchQuery");

const filterOptions = [
  { value: "all", label: "Все" },
  { value: "unread", label: "Непрочитанные" },
  { value: "read", label: "Прочитанные" },
  { value: "favorite", label: "Избранные" },
];

const total = computed(() => props.books.length);
const completed = computed(() => props.books.filter((b) => b.completed).length);
const favorite = computed(() => props.books.filter((b) => b.favorite).length);
</script>
