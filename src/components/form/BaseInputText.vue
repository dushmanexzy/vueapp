<template>
  <div>
    <label :for="idName">{{ label }}</label>
    <input
      type="text"
      :id="idName"
      :value="value"
      v-on:input="$emit('input', $event.target.value)"
    >
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: String,
      default: '',
    },
    idName: {
      type: String,
      required: true,
    },
    label: {
      type: String,
      required: true,
    },
  },
};
</script>

<style scoped>
input {
  border-radius: 5px;
  border: 2px solid grey;
  padding: 5px;
  width: 70%;
  outline: none;
}
</style>
