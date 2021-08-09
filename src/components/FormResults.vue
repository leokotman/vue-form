<template>
  <section>
    <h2>Submitted Results</h2>
    <button @click="loadData">Load data</button>
    <p v-if="isLoading">Loading...</p>
    <p v-else-if="!isLoading && error">
      {{ error }}
    </p>
    <p v-else-if="!isLoading && (!results || results.length === 0)">
      No stored data found. Save something to see the data.
    </p>
    <ul v-else-if="!isLoading && results && results.length > 0">
      <li v-for="result in results" :key="result.id">
        <h4>{{ result.userName }}</h4>
        <span>{{ result.userAge }}</span>
        <p>{{ result.infoSource }}</p>
        <ul>
          <li v-for="interest in result.interests" :key="interest[index]">
            <p>{{ interest }}</p>
          </li>
        </ul>
        <p>{{ result.howLearn }}</p>
        <p>{{ result.rating }}</p>
      </li>
    </ul>
  </section>
</template>

<script>
export default {
  // props: ["results"],
  data() {
    return {
      results: [],
      isLoading: false,
      error: null,
    };
  },
  methods: {
    loadData() {
      this.isLoading = true;
      fetch(
        "https://vue-form-a6941-default-rtdb.europe-west1.firebasedatabase.app/learning.json"
      )
        .then((response) => {
          if (response.ok) {
            return response.json();
          }
        })
        .then((data) => {
          this.isLoading = false;
          const results = [];
          for (const id in data) {
            results.push({
              id: id,
              userName: data[id].userName,
              userAge: data[id].userAge,
              infoSource: data[id].infoSource,
              interests: data[id].interests,
              howLearn: data[id].howLearn,
              rating: data[id].rating,
            });
          }
          this.results = results;
        })
        .catch((error) => {
          this.isLoading = false;
          console.error(error);
          this.error = "Something went wrong, please try again later";
        });
    },
  },
  mounted() {
    this.loadData();
  },
};
</script>

<style scoped>
ul {
  margin: 0;
  padding: 0;
}
ul li {
  display: flex;
  align-items: center;
  width: 40%;
}
li h4,
li span,
li p {
  margin: 1rem;
}
</style>
