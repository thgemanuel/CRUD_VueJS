<template>
  <div>
    <Banner />
    <div class="main-container">
      <h1>Monte o seu burguer:</h1>
      <BurguerForms />
    </div>
  </div>
</template>

<script>
import Banner from "../components/Banner.vue";
import BurguerForms from "../components/BurguerForms.vue";

export default {
  name: "Home",
  components: {
    Banner,
    BurguerForms,
  },
};
</script>
