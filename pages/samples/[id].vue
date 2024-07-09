<template>
  <div>
    <h1>Samples for Project ID: {{ project_id }}</h1>
    <table>
      <thead>
        <tr>
          <th>Sample ID</th>
          <th>External Sample ID</th>
          <th>External Patient ID</th>
          <th>External Sample URL</th>
          <th v-for="key in metadataKeys" :key="key">{{ key }}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="sample in samples" :key="sample.id">
          <td>{{ sample.id }}</td>
          <td>{{ sample.ext_sample_id }}</td>
          <td>{{ sample.patient.ext_patient_id }}</td>
          <td>{{ sample.ext_sample_url }}</td>
          <td v-for="key in metadataKeys" :key="key">
            {{ getMetadataValue(sample.metadata, key) }}
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
import { ref, computed } from 'vue'
import { useRoute } from 'vue-router'
import { useAsyncData } from 'nuxt/app'

const route = useRoute()
const sample_id = parseInt(route.params.id);
const project_id = route.query.project_id;

const { data: samples, error } = await useAsyncData('samples', () =>
  fetch(`http://localhost:8888/samples/${sample_id}?project_id=${project_id}`).then(res => {
    if (!res.ok) {
      throw new Error('Network response was not ok')
    }
    return res.json()
  })
)

const metadataKeys = computed(() => {
  const keys = new Set()
  if (samples.value) {
    samples.value.forEach(sample => {
      sample.metadata.forEach(meta => {
        keys.add(meta.key)
      })
    })
  }
  return Array.from(keys)
})

const getMetadataValue = (metadata, key) => {
  const meta = metadata.find(meta => meta.key === key)
  return meta ? meta.value : ''
}

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
</style>

