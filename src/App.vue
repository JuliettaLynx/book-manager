<template>
  <div class="container py-4">
    <header class="text-center mb-4">
      <h1 class="display-4 fw-bold text-primary">Менеджер книг</h1>
      <p class="lead text-secondary">Управляй своей библиотекой</p>
    </header>

    <main class="bg-white p-4 rounded-3 shadow-sm">
      <AddBookForm @add-book="addBook" />

      <BookFilters
        v-model:searchQuery="searchQuery"
        v-model:filter="currentFilter"
        :books="books"
      />

      <div v-if="filteredBooks.length === 0" class="text-center py-5">
        <p class="display-1 text-muted mb-3">📚</p>
        <p class="h4 text-muted mb-2">Книги не найдены :(</p>
        <p class="text-muted">
          Добавьте первую книгу или измените параметры поиска
        </p>
      </div>

      <div v-else class="row g-3 mt-2">
        <div
          v-for="book in filteredBooks"
          :key="book.id"
          class="col-12 col-md-6 col-lg-4"
        >
          <BookCard
            :book="book"
            @toggle="toggleBook(book.id)"
            @delete="deleteBook(book.id)"
            @rate="rateBook(book.id, $event)"
            @favorite="toggleFavorite(book.id)"
          />
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref, computed, watch } from "vue";
import AddBookForm from "./components/AddBookForm.vue";
import BookFilters from "./components/BookFilters.vue";
import BookCard from "./components/BookCard.vue";

const books = ref([]);

const savedBooks = localStorage.getItem("books");
if (savedBooks) {
  books.value = JSON.parse(savedBooks);
}

const currentFilter = ref("all");
const searchQuery = ref("");

watch(
  books,
  (newBooks) => {
    localStorage.setItem("books", JSON.stringify(newBooks));
  },
  { deep: true },
);

const addBook = (bookData) => {
  const newBook = {
    id: Date.now(),
    ...bookData,
    completed: false,
    rating: 0,
    favorite: false,
  };
  books.value.push(newBook);
};

const toggleBook = (id) => {
  const book = books.value.find((b) => b.id === id);
  if (book) {
    book.completed = !book.completed;
    if (!book.completed) {
      book.rating = 0;
    }
  }
};

const rateBook = (id, rating) => {
  const book = books.value.find((b) => b.id === id);
  if (book && book.completed) {
    book.rating = rating;
  }
};

const toggleFavorite = (id) => {
  const book = books.value.find((b) => b.id === id);
  if (book) {
    book.favorite = !book.favorite;
  }
};

const deleteBook = (id) => {
  if (confirm("Удалить книгу?")) {
    books.value = books.value.filter((b) => b.id !== id);
  }
};

const filteredBooks = computed(() => {
  return books.value
    .filter((book) => {
      if (currentFilter.value === "unread") return !book.completed;
      if (currentFilter.value === "read") return book.completed;
      if (currentFilter.value === "favorite") return book.favorite;
      return true;
    })
    .filter((book) => {
      if (!searchQuery.value) return true;
      const query = searchQuery.value.toLowerCase();
      return (
        book.title.toLowerCase().includes(query) ||
        book.author.toLowerCase().includes(query)
      );
    });
});
</script>

<style>
@import "bootstrap/dist/css/bootstrap.min.css";

body {
  background: linear-gradient(135deg, #667eea15 0%, #764ba215 100%);
  min-height: 100vh;
}
</style>
