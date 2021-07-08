<template>
  <the-form @submit-form="storeFormData"></the-form>
  <form-results
    :results="savedFormData"
    v-if="savedFormData.length !== 0"
  ></form-results>
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
      savedFormData: [],
    };
  },
  methods: {
    storeFormData(data) {
      const savedData = {
        id: new Date().toISOString(),
        userName: data.userName,
        userAge: data.userAge,
        infoSource: data.infoSource,
        interests: data.interests,
        howLearn: data.howLearn,
        rating: data.rating,
      };
      this.savedFormData.push(savedData);
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
}
</style>
