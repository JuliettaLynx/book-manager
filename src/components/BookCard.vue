<template>
  <div class="card h-100 shadow-sm" :class="{ 'bg-light': book.completed }">
    <div class="card-body">
      <div class="d-flex justify-content-between align-items-start mb-2">
        <h5 class="card-title mb-0">{{ book.title }}</h5>
        <span
          @click="toggleFavorite"
          class="fs-4 text-danger"
          style="cursor: pointer"
        >
          {{ book.favorite ? "❤️" : "🤍" }}
        </span>
      </div>

      <p class="card-text text-muted mb-2">{{ book.author }}</p>
      <span class="badge bg-secondary mb-3">{{ book.genre }}</span>

      <div v-if="book.completed" class="mb-3">
        <div class="d-flex gap-1">
          <span
            v-for="star in 5"
            :key="star"
            @click="$emit('rate', star)"
            class="fs-5"
            style="cursor: pointer"
            :class="star <= book.rating ? 'text-warning' : 'text-secondary'"
          >
            {{ star <= book.rating ? "★" : "☆" }}
          </span>
        </div>
      </div>

      <div class="d-flex gap-2">
        <button
          @click="$emit('toggle')"
          :class="[
            'btn',
            book.completed ? 'btn-info' : 'btn-success',
            'btn-sm',
            'flex-grow-1',
          ]"
        >
          {{ book.completed ? "Прочитано" : "Отметить прочитанной" }}
        </button>

        <button @click="$emit('delete')" class="btn btn-danger btn-sm">
          ✕
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps(["book"]);
const emit = defineEmits(["toggle", "delete", "rate", "favorite"]);

const toggleFavorite = () => {
  emit("favorite");
};
</script>
