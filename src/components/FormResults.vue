<template>
  <section>
    <base-card>
      <h2>Submitted Results</h2>
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
    </base-card>
  </section>
</template>

<script>
export default {
  props: ["results"],
};
</script>

<style scoped></style>
