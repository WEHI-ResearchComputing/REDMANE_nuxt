<template>
  <div>
    <h1>Patients</h1>
  </div>
  <div>
    <table class="table table-striped" v-if="patients && patients.length > 0">
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
          <td>
                <nuxt-link :to="'/patients/view/'+patient.id+'?project_id='+project_id">
                    {{ patient.ext_patient_id }}
                </nuxt-link>
          </td>
          <td>{{ patient.ext_patient_url }}</td>
          <td>{{ patient.sample_count }}</td>
        </tr>
      </tbody>
    </table>
    <p v-else-if="patients && patients.length === 0">No patients found.</p>
    <p v-else>Loading...</p>
  </div>
</template>

<script setup>
import { useAsyncData } from 'nuxt/app';
import { useRoute } from 'vue-router';

const route = useRoute();
const patient_id = parseInt(route.params.id);
const project_id = route.query.project_id;


const { data: patients, error } = useAsyncData('patients', async () => {
  const response = await fetch(`http://localhost:8888/patients/${patient_id}?project_id=${project_id}`);
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
