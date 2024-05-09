<script setup>
import { ref, onMounted, computed, watch } from "vue";

const todos = ref([]);
const name = ref("");

const input_content = ref("");
const input_categories = ref(null);

const todos_asc = computed(() =>
  todos.value.sort((a, b) => {
    return b.createdAt - a.createdAt;
  })
);

const addtodo = () => {
  if (input_content.value.trim() === "" || input_category.value === null) {
    return;
  }
   
  todos.value.push({
    content: input_content.value,
    category: input_categories.value,
    done: false,
    createdAt: new Date.getTime()
  });
 };



</script>
<template>
  <main class="app">
    <section class="container">
      <h3>TODO</h3>
      <Form @submit.prevent="addtodo">
        <input
          type="text"
          placeholder="e.g make a video"
          v-model="input_content"
        />
      </Form>
    </section>

    <section class="create-todo">
      <div>
        <h4>Pick a category</h4>
      </div>

      <div class="options">
        <label>
          <input
            type="radio"
            name="category"
            value="business"
            v-model="input_category"
          />
          <span class="bubble business"></span>
          <div>Business</div>
        </label>

        <label>
          <input
            type="radio"
            name="category"
            value="personal"
            v-model="input_category"
          />
          <span class="bubble personal"></span>
          <div>Personal</div>
        </label>
      </div>

      <input type="submit" value="Add todo" />
    </section>

    <section class="todo-list">
      <h3>Todo-list</h3>
    </section>
  </main>
</template>
