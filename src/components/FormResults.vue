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
      <li v-for="result in results" :key="result.id" class="result">
        <h4>{{ result.userName }}</h4>
        <span>{{ result.userAge }}</span>
        <p>{{ result.infoSource }}</p>
        <ul>
          <li v-for="interest in result.interests" :key="interest">
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
  props: {
    formSubmitted: {
      type: Boolean,
    },
  },
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
  watch: {
    formSubmitted() {
      if (this.formSubmitted === true) {
        console.log("watcher worked - results updated");
        this.loadData();
      }
    },
  },
  mounted() {
    this.loadData();
  },
};
</script>

<style scoped>
section {
  margin: 1rem;
  min-width: 40%;
  max-width: 50%;
  height: max-content;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(24, 23, 116, 0.26);
  padding: 1rem;
  box-sizing: border-box;
  background-color: #ffffff;
}

ul {
  margin: 1rem 0 1rem;
  padding: 0;
}
ul li {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-between;
}
.result {
  border-radius: 8px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.26);
}

button {
  font: inherit;
  border: 1px solid #0076bb;
  background-color: #0076bb;
  color: white;
  cursor: pointer;
  padding: 0.75rem 2rem;
  border-radius: 30px;
}

button:hover,
button:active {
  border-color: #002350;
  background-color: #002350;
}
</style>
