<template>
  <div class="app">
    <h2>Посты</h2>

    <button @click="loadPosts">Загрузить посты</button>

    <ul v-if="posts.length">
      <li v-for="post in posts" :key="post.id">
        {{ post.title }}
      </li>
    </ul>

    <hr />

    <h2>Создать новый пост</h2>

    <form @submit.prevent="createPost">
      <div>
        <label>Заголовок:</label><br />
        <input v-model="form.title" required />
      </div>

      <div style="margin-top: 10px;">
        <label>Текст:</label><br />
        <textarea v-model="form.body" required></textarea>
      </div>

      <button style="margin-top: 10px;" type="submit">Отправить</button>
    </form>

    <p v-if="successMessage" style="color: green; margin-top: 10px;">
      {{ successMessage }}
    </p>
  </div>
</template>

<script setup>
import { ref } from "vue";

const posts = ref([]);
const successMessage = ref("");

const form = ref({
  title: "",
  body: ""
});

async function loadPosts() {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts");
  posts.value = await response.json();
}

async function createPost() {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts", {
    method: "POST",
    headers: {
      "Content-Type": "application/json"
    },
    body: JSON.stringify(form.value)
  });

  const newPost = await response.json();

  successMessage.value = "Пост успешно создан!";
  form.value.title = "";
  form.value.body = "";

  posts.value.unshift(newPost);
}
</script>

<style>
body {
  font-family: sans-serif;
}
.app {
  padding: 20px;
  max-width: 500px;
}
</style>