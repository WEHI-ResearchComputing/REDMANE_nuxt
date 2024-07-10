<template>
  <div>
    <h1>Patient</h1>
  </div>
  <div>
      <div v-if="patients && patients.length > 0">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>ID</th>
              <th>External ID</th>
              <th>External Source</th>
              <th>Number of samples</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="patient in patients" :key="patient.id">
              <td>{{ patient.id }}</td>
              <td>{{ patient.ext_patient_id }}</td>
              <td>{{ patient.ext_patient_url }}</td>
              <td>{{ patient.sample_count }}</td>
            </tr>
          </tbody>
        </table>


 
     </div>

    <p v-else-if="patients && patients.length === 0">No patients found.</p>
    <p v-else>Loading...</p>
  </div>

    <!-- Show metadata as a table -->
    <div v-if="patients[0]">
      <h2>Metadata</h2>
      <table class="table table-striped">
        <thead>
          <tr>
            <th>Key</th>
            <th>Value</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in patients[0].metadata" :key="item.id">
            <td>{{ item.key }}</td>
            <td>{{ item.value }}</td>
          </tr>
        </tbody>
      </table>
    </div>

    <!-- Show samples as a table -->
    <div v-if="patients[0] && patients[0].samples.length > 0">
        <h2>Samples</h2>
        <table class="table table-striped">
          <thead>
            <tr>
              <th>Sample ID</th>
              <th>External Sample ID</th>
              <th>External Sample URL</th>
              <th>Metadata</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="sample in patients[0].samples" :key="sample.id">
              <td>{{ sample.id }}</td>
              <td>{{ sample.ext_sample_id }}</td>
              <td>{{ sample.ext_sample_url }}</td>
              <td>
                <ul>
                  <li v-for="meta in sample.metadata" :key="meta.id">
                    {{ meta.key }}: {{ meta.value }}
                  </li>
                </ul>
              </td>
            </tr>
          </tbody>
        </table>
    </div>

</template>

<script setup>
import { useAsyncData } from 'nuxt/app';
import { useRoute } from 'vue-router';

const route = useRoute();
const patient_id = parseInt(route.params.id);
const project_id = route.query.project_id;


const { data: patients, error } = useAsyncData('patients', async () => {
  const response = await fetch(`http://localhost:8888/patients_metadata/${patient_id}?project_id=${project_id}`);
  if (!response.ok) {
    throw new Error('Network response was not ok');
  }
  return response.json();
});

if (error.value) {
  console.error('Error fetching patients:', error.value);
}
</script>

<style scoped>
/* Optional: Add scoped style for specific page styling */
</style>
