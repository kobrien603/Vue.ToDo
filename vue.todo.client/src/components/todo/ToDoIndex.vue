<template>
  <div class="todo-list">
    <h2>Todo List</h2>

    <LoadingContainer v-if="loading" />

    <div v-if="todos.length === 0 && !loading">No todos found.</div>

    <div v-else-if="todos.length > 0">
      <ul>
        <li v-for="todo in todos" :key="todo.id">
          <input type="checkbox" v-model="todo.completed" />
          <span :class="{ completed: todo.completed }">{{ todo.description }}</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import type { ToDo } from '../../types/ToDo';
import LoadingContainer from '../shared/LoadingContainer.vue';

export default defineComponent({
  components: {
    LoadingContainer,
  },
  data() {
    return {
      loading: true,
      todos: [] as ToDo[],
    }
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    async fetchData() {
      try {
        const response = await fetch('todos');
        if (response.ok) {
          this.todos = await response.json();
        }
      } catch (error) {
        console.error('Error fetching todos:', error);
      } finally {
        this.loading = false;
      }
    },
    toggleTodoStatus(todo: ToDo) {
      todo.completed = !todo.completed;
    }
  },
});
</script>
