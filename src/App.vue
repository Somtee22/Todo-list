<script setup>
import { ref, onMounted, computed, watch } from "vue";

const todos = ref([]);

const input_content = ref("");
const input_category = ref(null);

const todos_asc = computed(() =>
  todos.value.sort((a, b) => {
    return b.createdAt - a.createdAt;
  })
);

const addTodo = () => {
  if (input_content.value.trim() === "" || input_category.value === null) {
    return;
  }

  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime(),
  });
};

 const deleteTodo = todo => {
  todos.value = todos.value.filter(t => t !== todo) 
};

watch(
  todos,
  (newValue) => {
    localStorage.setItem("todos", JSON.stringify(newValue));
  },
  { deep: true }
);

onMounted(() => {
  todos.value = JSON.parse(localStorage.getItem("todos")) || [];
});
</script>



<template>
  <main class="App">
    <section class="container">
      <h3>TODO</h3>
      <form @submit.prevent="addTodo">
        <input
          type="text"
          placeholder="e.g watch a Vue course on udemy"
          v-model="input_content"
        />
      </form>
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

      <button class="btn" @click="addTodo">Add Todo</button>
    </section>

    <section class="todo-list">
      <h4>TODO LIST</h4>
      <div class="list">
        <div
          v-for="todo in todos_asc"
          :class="`todo-item ${todo.done && 'done'}`"
        >

        <label>
          <input
            type="checkbox"
            v-model="todo.done"
          />
          <span :class="`bubble ${todo.category}` "></span>
        </label>
     
        <div class="todo-content">
          <input type="text" v-model="todo.content" />
        </div>
 
        <div class="actions">
          <button class="delete" @click="deleteTodo(todo)">Delete</button>
        </div>

      </div>
      </div>
    </section>
  </main>
</template>
