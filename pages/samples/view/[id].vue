<template>
  <div>
    <h1>Sample Details</h1>
    <div v-if="sample">
      <div>
        <h2>Sample Information</h2>
        <p>ID: {{ sample.id }}</p>
        <p>External ID: {{ sample.ext_sample_id }}</p>
        <p>External Source: {{ sample.ext_sample_url }}</p>
      </div>

      <div v-if="sample.metadata.length > 0">
        <h2>Sample Metadata</h2>
        <table class="table table-striped">
          <thead>
            <tr>
              <th>Key</th>
              <th>Value</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in sample.metadata" :key="item.id">
              <td>{{ item.key }}</td>
              <td>{{ item.value }}</td>
            </tr>
          </tbody>
        </table>
      </div>

      <div>
        <h2>Patient Information</h2>
        <p>ID: {{ sample.patient.id }}</p>
        <p>External ID: <nuxt-link :to="'/patients/view/'+sample.patient.id+'?project_id='+project_id">{{ sample.patient.ext_patient_id }}</nuxt-link></p>
        <p>External Source: {{ sample.patient.ext_patient_url }}</p>
        <p v-if="sample.patient.public_patient_id">Public Patient ID: {{ sample.patient.public_patient_id }}</p>
      </div>
    </div>
    <p v-else>Loading...</p>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';

const route = useRoute();
const sample_id = parseInt(route.params.id);
const project_id = route.query.project_id;

const sample = ref(null);
const error = ref(null);

onMounted(async () => {
  try {
    const response = await fetch(`http://localhost:8888/samples/${sample_id}?project_id=${project_id}`);
    if (!response.ok) throw new Error('Network response was not ok');
    const data = await response.json();
    sample.value = data[0];
  } catch (err) {
    error.value = err;
    console.error('Error fetching sample details:', error.value);
  }
});
</script>

<style scoped>
/* Optional: Add scoped style for specific page styling */
</style>

