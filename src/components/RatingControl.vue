<template>
  <ul>
    <li :class="{ active: modelValue === 'bad' }">
      <button type="button" @click="activateBtn('bad')">Bad</button>
    </li>
    <li :class="{ active: modelValue === 'normal' }">
      <button type="button" @click="activateBtn('normal')">Normal</button>
    </li>
    <li :class="{ active: modelValue === 'great' }">
      <button type="button" @click="activateBtn('great')">Great</button>
    </li>
  </ul>
</template>

<script>
export default {
  //for working v-model in custom component
  props: ["modelValue"],
  emits: ["update:modelValue"],

  methods: {
    activateBtn(option) {
      this.$emit("update:modelValue", option);
    },
  },
};
</script>

<style scoped>
ul {
  list-style-type: none;
  display: flex;
  justify-content: center;
  align-items: center;
}
ul li {
  margin: 0 1rem;
  border: 1px solid #ccc;
  border-radius: 5px;
  display: flex;
}
li button {
  padding: 1rem;
  cursor: pointer;
  border: none;
  background-color: transparent;
}

.active {
  border: 3px solid slateblue;
}
.active button {
  color: indigo;
}
</style>
