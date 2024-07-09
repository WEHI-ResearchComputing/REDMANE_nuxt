<template>
  <div>
    <h1>Samples for Project ID: {{ projectId }}</h1>
    <ul>
      <li v-for="sample in samples" :key="sample.id">
        <p>Sample ID: {{ sample.id }}</p>
        <p>External Sample ID: {{ sample.ext_sample_id }}</p>
        <p>External Sample URL: <a :href="sample.ext_sample_url" target="_blank">{{ sample.ext_sample_url }}</a></p>
        <div v-if="sample.metadata.length">
          <h4>Metadata:</h4>
          <ul>
            <li v-for="meta in sample.metadata" :key="meta.id">
              {{ meta.key }}: {{ meta.value }}
            </li>
          </ul>
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import { useRoute } from 'vue-router'
import { useFetch } from 'nuxt/app'

const route = useRoute()
const projectId = parseInt(route.params.id);


const { data: samples, error } = useAsyncData('samples', async () => {
  const response = await fetch(`http://localhost:8888/samples/${projectId}?patient_id=0`);
  if (!response.ok) {
    throw new Error('Network response was not ok');
  }
  return response.json();
});
</script>

<style scoped>
/* Add any necessary styles here */
</style>

