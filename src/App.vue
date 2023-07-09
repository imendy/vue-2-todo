

 <template>
  <div>
    <Header />
    <router-view />
  </div>
</template>

<script>
import Header from './components/Header.vue';

export default {
  components: {
    Header
  }
};
</script>

<style>
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  padding-top: 80px; /* Add padding-top to create space for the fixed header */
}
/* Rest of the styles */
</style>

