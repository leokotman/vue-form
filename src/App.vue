<template>
  <the-form @submit-form="updateResults"></the-form>
  <form-results :formSubmitted="formSubmitted"></form-results>
</template>

<script>
import TheForm from "./components/TheForm.vue";
import FormResults from "./components/FormResults.vue";

export default {
  name: "App",
  components: {
    TheForm,
    FormResults,
  },
  data() {
    return {
      formSubmitted: false,
    };
  },
  methods: {
    updateResults() {
      this.formSubmitted = true;
      console.log("new data submitted");
      setTimeout(() => {
        this.formSubmitted = false;
        console.log("formSubmitted = false");
      }, 3000);
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  display: flex;
}
</style>
