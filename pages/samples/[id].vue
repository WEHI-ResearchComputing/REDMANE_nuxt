<template>
  <div>
    <h1>Samples for Project ID: {{ projectId }}</h1>
    <table>
      <thead>
        <tr>
          <th>Sample ID</th>
          <th>External Sample ID</th>
          <th>External Patient ID</th>
          <th>External Sample URL</th>
          <th>Metadata</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="sample in samples" :key="sample.id">
          <td>{{ sample.id }}</td>
          <td>{{ sample.ext_sample_id }}</td>
          <td>{{ sample.patient.ext_patient_id }}</td>
          <td>{{ sample.ext_sample_url }}</td>
          <td>
            <ul>
              <li v-for="meta in sample.metadata" :key="meta.id">
                <strong>{{ meta.key }}:</strong> {{ meta.value }}
              </li>
            </ul>
          </td>
        </tr>
      </tbody>
    </table>
    <div v-if="error">
      <p>Error fetching samples: {{ error.message }}</p>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'
import { useRoute } from 'vue-router'
import { useAsyncData } from 'nuxt/app'

const route = useRoute()
const projectId = computed(() => parseInt(route.params.id, 10))

const { data: samples, error } = await useAsyncData('samples', () =>
  fetch(`http://localhost:8888/samples/${projectId.value}?patient_id=0`).then(res => {
    if (!res.ok) {
      throw new Error('Network response was not ok')
    }
    return res.json()
  })
)

if (error.value) {
  console.error('Error fetching samples:', error.value)
}
</script>

<style scoped>
table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  border: 1px solid #ddd;
  padding: 8px;
}

th {
  background-color: #f2f2f2;
}

td ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
}

td ul li {
  margin: 0;
}
</style>

