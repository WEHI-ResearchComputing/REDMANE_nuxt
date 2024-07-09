<template>
  <div>
    <ul v-if="datasets">
      <li v-for="dataset in datasets" :key="dataset.id">
        {{ dataset.name }}
      </li>
    </ul>
    <p v-else>Loading...</p>
  </div>
</template>

<script setup>
import { useAsyncData } from 'nuxt/app';
import { useRoute } from 'vue-router';

const route = useRoute();
const projectId = parseInt(route.params.id);

const { data: datasets, error } = useAsyncData('datasets', async () => {
  const response = await fetch(`http://localhost:8888/datasets/${projectId}`);
  if (!response.ok) {
    throw new Error('Network response was not ok');
  }
  return response.json();
});

if (error.value) {
  console.error('Error fetching datasets:', error.value);
}
</script>

