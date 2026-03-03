<template>
  <div class="card mb-4 shadow-sm">
    <div class="card-body">
      <h2 class="card-title h4 mb-3">Добавить новую книгу</h2>

      <form @submit.prevent="handleSubmit">
        <div class="mb-3">
          <input
            v-model="formData.title"
            type="text"
            class="form-control"
            placeholder="Название книги"
            required
          />
        </div>

        <div class="mb-3">
          <input
            v-model="formData.author"
            type="text"
            class="form-control"
            placeholder="Автор"
            required
          />
        </div>

        <div class="mb-3">
          <select v-model="formData.genre" class="form-select" required>
            <option value="">Выберите жанр</option>
            <option value="Роман">Роман</option>
            <option value="Фантастика">Фантастика</option>
            <option value="Детектив">Детектив</option>
            <option value="Научная">Научная</option>
            <option value="Поэзия">Поэзия</option>
          </select>
        </div>

        <button type="submit" class="btn btn-success w-100">
          <i class="bi bi-plus-circle me-2"></i>Добавить книгу
        </button>
      </form>
    </div>
  </div>
</template>

<script setup>
import { reactive } from "vue";

const emit = defineEmits(["add-book"]);

const formData = reactive({
  title: "",
  author: "",
  genre: "",
});

const handleSubmit = () => {
  emit("add-book", { ...formData });
  formData.title = "";
  formData.author = "";
  formData.genre = "";
};
</script>
