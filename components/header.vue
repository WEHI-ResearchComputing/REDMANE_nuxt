<template>
  <header class="header">
    <nav class="navbar">
      <div class="navbar-brand">
        <NuxtLink to="/projects" class="navbar-item">
        </NuxtLink>
      </div>
      <div class="navbar-menu">
        <div class="navbar-start">
          <NuxtLink to="/projects" class="navbar-item">Home</NuxtLink>
        </div>
      </div>
    </nav>
  </header>
</template>

<script setup>
</script>

<style scoped>
.header {
  background-color: #333;
  color: white;
  padding: 1rem;
}

.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.navbar-brand .navbar-item img {
  max-height: 50px;
}

.navbar-menu .navbar-start .navbar-item {
  margin-right: 1rem;
  color: white;
  text-decoration: none;
}

.navbar-menu .navbar-start .navbar-item:hover {
  text-decoration: underline;
}
</style>

