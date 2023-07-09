<template>
  <!-- The header component -->
  <header>
    <!-- The brand name of the app -->
    <h1>{{ brandName }}</h1>
    <nav>
      <!-- Router links for navigation -->
      <router-link to="/" class="nav-link">Home</router-link>
      <router-link to="/about" class="nav-link">About</router-link>
    </nav>
  </header>
</template>

<script>
export default {
  data() {
    return {
      // The brand name variable
      brandName: 'TodoPro'
    };
  }
};
</script>

<style>
/* Styles for the header component */
header {
  /* Fixed position at the top of the screen */
  position: fixed;
  top: 0;
  right: 0;
  left: 0;
  background-color: #008CBA;
  color: #fff;
  padding: 20px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

h1 {
  font-size: 14px;
  margin: 0;
}

nav {
  margin-top: 10px;
}

.nav-link {
  color: #fff;
  text-decoration: none;
  margin-right: 10px;
  font-size: 11px;
  transition: transform 0.3s ease;
}

.nav-link:hover {
  /* Hover effect for navigation links */
  opacity: 50%;
}

@media (min-width: 560px) {
  header {
    padding: 20px 32px;
  }
}

@media (min-width: 720px) {
  header {
    padding: 1.2rem 8rem;
  }
}

@media (min-width: 920px) {
  header {
    padding: 1.2rem 12rem;
  }
}

/* Rest of the styles */
</style>
