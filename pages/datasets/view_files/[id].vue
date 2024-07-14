<template>
  <div>
    <h1>Dataset Details</h1>

    <div v-if="files && files.length > 0">
      <table class="table table-striped">
        <thead>
          <tr>
            <th>ID</th>
            <th>Path</th>
            <th>Sample ID</th>
            <th>External Sample ID</th>
            <th v-for="key in allMetadataKeys" :key="key">{{ key }}</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="file in files" :key="file.id">
            <td>{{ file.id }}</td>
            <td>{{ file.path }}</td>
            <td>{{ file.sample_id }}</td>
            <td>{{ file.ext_sample_id }}</td>
            <td v-for="key in allMetadataKeys" :key="key">
              {{ getMetadataValue(file, key) }}
            </td>
          </tr>
        </tbody>
      </table>
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

const files = ref([]);
const allMetadataKeys = ref([]);

// Fetch dataset details and metadata from API
onMounted(async () => {
  try {
    const response = await fetch(`http://localhost:8888/raw_files_with_metadata/${datasetId}?project_id=${projectId}`);
    if (!response.ok) {
      throw new Error('Failed to fetch dataset');
    }
    const data = await response.json();
    files.value = data; // Assuming your JSON structure directly provides files array
    collectMetadataKeys();
  } catch (error) {
    console.error('Error fetching dataset:', error);
  }
});

// Function to collect all unique metadata keys
function collectMetadataKeys() {
  const keys = new Set();
  files.value.forEach(file => {
    file.sample_metadata.forEach(metadata => {
      keys.add(metadata.key);
    });
  });
  allMetadataKeys.value = Array.from(keys);
}

// Function to get metadata value for a given key
function getMetadataValue(file, key) {
  const metadata = file.sample_metadata.find(item => item.key === key);
  return metadata ? metadata.value : '';
}
</script>

<style scoped>
/* Optional: Add scoped style for specific page styling */
</style>

