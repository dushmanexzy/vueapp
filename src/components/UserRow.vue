<template>
  <tr>
    <td>
      <span
        @click="toggleSubordinates(user)"
        v-if="user.subordinates && user.subordinates.length > 0"
      >
        {{ user.isExpanded ? '-' : '+' }}
      </span>
      {{ user.name }}
    </td>
    <td>{{ user.phone }}</td>
  </tr>
</template>

<script>
export default {
  name: 'UserRow',
  props: {
    user: {
      type: Object,
      required: true,
    },
    users: {
      type: Array,
    },
  },
  methods: {
    toggleSubordinates(user) {
      if (user.subordinates) {
        const copyUser = { ...user };
        copyUser.isExpanded = !copyUser.isExpanded;
        const index = this.users.findIndex(u => u.id === user.id);
        if (index !== -1) {
          this.users.splice(index, 1, copyUser);
        }
      }
    },
  },
};
</script>

<style scoped>
th,
td {
  border: 1px solid grey;
  padding: 1rem;
  text-align: left;
}
</style>
