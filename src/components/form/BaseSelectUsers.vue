<template>
  <div>
    <label :for="idName">{{ label }}</label>
    <select
      :id="idName"
      :value="value"
      v-on:input="$emit('input', $event.target.value)"
    >
      <option value="" selected></option>
      <option v-for="user in allUsers" :key="user.id" :value="user.name">
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
    users: {
      type: Array,
      required: true,
      default: JSON.parse(localStorage.getItem('users')) || [],
    },
  },
  data() {
    return {
      allUsers: [],
    };
  },
  created() {
    this.allUsers = this.getAllUsers(this.users);
  },
  methods: {
    getAllUsers(users) {
      return users.reduce((allUsers, user) => {
        allUsers.push(user);
        if (user.subordinates && user.subordinates.length > 0) {
          allUsers.push(...this.getAllUsers(user.subordinates));
        }
        return allUsers;
      }, []);
    },
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
