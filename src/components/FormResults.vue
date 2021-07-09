<template>
  <section>
    <h2>Submitted Results</h2>
    <button @click="loadData">Load data</button>
    <ul>
      <li v-for="result in results" :key="result.id">
        <h4>{{ result.userName }}</h4>
        <span>{{ result.userAge }}</span>
        <p>{{ result.infoSource }}</p>
        <p>{{ result.interests }}</p>
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
    };
  },
  methods: {
    loadData() {
      fetch("https://vue-form-http-default-rtdb.firebaseio.com/learning.json")
        .then((response) => {
          if (response.ok) {
            return response.json();
          }
        })
        .then((data) => {
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
        });
    },
  },
};
</script>

<style scoped></style>
