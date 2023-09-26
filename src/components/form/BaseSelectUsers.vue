<template>
  <div>
    <label :for="idName">{{ label }}</label>
    <select
      :id="idName"
      :value="value"
      v-on:input="$emit('input', $event.target.value)"
    >
      <option value="" selected></option>
      <option v-for="user in users" :value="user.name" :key="user.id">
        {{ user.name }}
      </option>
    </select>
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
  data() {
    return {
      users: JSON.parse(localStorage.getItem('users')) || [],
    };
  },
};
</script>

<style scoped>
select {
  border-radius: 5px;
  border: 2px solid grey;
  padding: 5px;
  width: 70%;
  outline: none;
}
</style>
