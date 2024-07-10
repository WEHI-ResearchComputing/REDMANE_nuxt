<template>
  <div>
    <h1>Datasets</h1>
  </div>
  <div>
    <table class="table table-striped" v-if="datasets && datasets.length > 0">
      <thead>
        <tr>
          <th>ID</th>
          <th>Name</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="dataset in datasets" :key="dataset.id">
          <td>{{ dataset.id }}</td>
          <td><nuxt-link :to="'/datasets/view/'+dataset.id+'?project_id='+project_id">{{ dataset.name }}</nuxt-link></td>
        </tr>
      </tbody>
    </table>
    <p v-else-if="datasets && datasets.length === 0">No datasets found.</p>
    <p v-else>Loading...</p>
  </div>
</template>

<script setup>
import { useAsyncData } from 'nuxt/app';
import { useRoute } from 'vue-router';

const route = useRoute();
const dataset_id = parseInt(route.params.id);
const project_id = route.query.project_id;

const { data: datasets, error } = useAsyncData('datasets', async () => {
  const response = await fetch(`http://localhost:8888/datasets/${dataset_id}?project_id=${project_id}`);
  if (!response.ok) {
    throw new Error('Network response was not ok');
  }
  return response.json();
});

if (error.value) {
  console.error('Error fetching datasets:', error.value);
}
</script>

<style scoped>
/* Optional: Add scoped style for specific page styling */
</style>

