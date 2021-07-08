<template>
  <the-form @submit-form="storeFormData"></the-form>
  <p>{{ savedFormData }}</p>
</template>

<script>
import TheForm from "./components/TheForm.vue";

export default {
  name: "App",
  components: {
    TheForm,
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
