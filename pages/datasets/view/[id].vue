<template>
  <div>
    <h1>Dataset Details</h1>

    <div v-if="dataset">
      <h2>{{ dataset.name }}</h2>
      <p><strong>Project ID:</strong> {{ dataset.project_id }}</p>

      <h3>Metadata</h3>
      <table class="table table-striped">
        <thead>
          <tr>
            <th>Key</th>
            <th>Value</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="meta in dataset.metadata" :key="meta.id">
            <td>{{ meta.key }}</td>
            <td>{{ meta.value }}</td>
          </tr>
        </tbody>
      </table>
      <nuxt-link :to="'/datasets/view_files/'+dataset.id+'?project_id='+project_id">View Files</nuxt-link>
    </div>
    <p v-else>Loading...</p>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';

const route = useRoute();
const datasetId = parseInt(route.params.id);
const projectId = parseInt(route.query.project_id);

const dataset = ref(null);

// Fetch dataset details and metadata from API
onMounted(async () => {
  try {
    const response = await fetch(`http://localhost:8888/datasets_with_metadata/${datasetId}?project_id=${projectId}`);
    if (!response.ok) {
      throw new Error('Failed to fetch dataset');
    }
    const data = await response.json();
    dataset.value = data;
  } catch (error) {
    console.error('Error fetching dataset:', error);
  }
});
</script>

<style scoped>
/* Optional: Add scoped style for specific page styling */
</style>

