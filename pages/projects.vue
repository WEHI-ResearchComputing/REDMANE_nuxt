<template>
  <div>
    <h1>Projects</h1>
    <table v-if="data" border="1">
      <thead>
        <tr>
          <th>ID</th>
          <th>Name</th>
          <th>Status</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="project in data" :key="project.id">
          <td>{{ project.id }}</td>
          <td>{{ project.name }}</td>
          <td>{{ project.status }}</td>
        </tr>
      </tbody>
    </table>
    <p v-else-if="error">Error fetching projects data: {{ error.message }}</p>
    <p v-else>Loading...</p>
  </div>
</template>

<script setup>
import { useAsyncData } from 'nuxt/app';

const { data, error } = await useAsyncData('projects', () => fetch('http://localhost:8888/projects')
  .then(response => {
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    return response.json();
  })
);

if (error.value) {
  console.error('Error fetching projects data:', error.value);
}
</script>

