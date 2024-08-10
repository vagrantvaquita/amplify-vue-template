<script setup lang="ts">
import '@/assets/main.css';
import { onMounted, ref } from 'vue';
import type { Schema } from '../../amplify/data/resource';
import { generateClient } from 'aws-amplify/data';

const client = generateClient<Schema>();

// create a reactive reference to the array of todos
const todos = ref<Array<Schema['Todo']["type"]>>([]);

async function listTodos() {
  const { data: items } = await client.models.Todo.list()
  todos.value = items
}

function deleteTodo(id: string) {
  client.models.Todo.delete({id})
  listTodos();
}

function createTodo() {
  console.log("eh")
  client.models.Todo.create({
    content: window.prompt("Todo content")
  }).then(() => {
    // After creating a new todo, update the list of todos
    // listTodos();
  });
}
    
// fetch todos when the component is mounted
 onMounted(() => {
  // listTodos();
});

</script>

<template>
  <main>
    <h1>My todos</h1>
    <button @click="createTodo">+ new</button>
    <ul>
      <li 
        v-for="todo in todos" 
        :key="todo.id"
        @click="deleteTodo(todo.id)"
        >
        {{ todo.content }}
      </li>
    </ul>
    <button @click="listTodos">+ Refresh</button>
    <div>
      🥳 App successfully hosted. Try creating a new todo.
      <br />
      <a href="https://docs.amplify.aws/gen2/start/quickstart/nextjs-pages-router/">
        Review next steps of this tutorial.
      </a>
    </div>
  </main>
</template>
