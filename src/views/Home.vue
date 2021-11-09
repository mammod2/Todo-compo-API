<template>
  <div class="home">
    <div>
      <h1>Todo app</h1>
      <!-- enter the value -->
      <input type="text" placeholder="Enter your todos" v-model="newTodo" />
      <!-- ratings -->
      <input
        type="number"
        placeholder="enter ratings"
        min="1"
        max="5"
        v-model="todoRating"
      />
      <button @click="handleSubmit">Submit</button>
      <button @click="handleUndo()">Undo</button>
    </div>
    <!-- todo values -->
    <div>
      <ul
        v-for="(todo, i) in filterTodos"
        :key="i"
        :class="{ done: todo.completed }"
      >
        <li>
          {{ todo.title }}
          <button @click="removeTodo(i)">X</button>
          <input
            type="checkbox"
            v-model="todo.completed"
            :class="{ done: todo.completed }"
          />
          <b>Rating :</b>{{ todo.rating }}
        </li>
      </ul>
      <select name="filter" v-model="filterRank">
        <option value="All">All</option>
        <option value="Deleted">Deleted</option>
        <option value="Completed">Completed</option>
        <option value="Sort">Sort</option>
      </select>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import { computed, ref } from "vue";
export default {
  name: "Home",
  setup() {
    const newTodo = ref("");
    const deletedTodo = ref([]);
    const filterRank = ref("All");
    const todoRating = ref("");
    const todos = ref([
      { title: "eat chicken nuggets", completed: false, rating: 1 },
      { title: "Learn compo", completed: false, rating: 2 },
      { title: "sleep", completed: false, rating: 5 },
    ]);
    // functions
    function handleSubmit() {
      todos.value.push({
        title: newTodo.value,
        rating: todoRating.value,
      });
      newTodo.value = "";
    }
    function removeTodo(i) {
      deletedTodo.value.push(todos.value[i]);
      todos.value.splice(i, 1);
    }

    // computed
    const filterTodos = computed(() => {
      if (filterRank.value === "All") return todos.value;
      else if (filterRank.value === "Deleted") return deletedTodo.value;
      else if (filterRank.value === "Completed") {
        return todos.value.filter((todo) => todo.completed === true);
      } else if (filterRank.value === "Sort") {
        return todos.value.sort((a, b) => a.rating - b.rating);
      }
    });
    function handleUndo() {
      deletedTodo.value.forEach((todo) => {
        todos.value.push(todo);
      });
      deletedTodo.value = [];
    }
    return {
      todos,
      newTodo,
      filterRank,
      todoRating,
      handleSubmit,
      removeTodo,
      handleUndo,
      filterTodos,
    };
  },
};
</script>
<style>
ul {
  cursor: pointer;
}
li {
  text-decoration: none;
}
.done {
  background: blue;
  /* box-shadow: 10px 10px 8px 10px #888888; */
}
</style>
