<template>
  <div class="app-left">
  <!--   Users Table   -->
    <table class="users-table">
      <!--    Head Table    -->
      <thead>
      <tr>
        <th @click="sortBy('name')" class="sort-by-key">Имя</th>
        <th @click="sortBy('phone')" class="sort-by-key">Телефон</th>
      </tr>
      </thead>
      <!--    Body Table    -->
      <tbody v-if="users.length > 0" v-for="user in users" :key="user.id">
        <UserRow :user="user" :users="users" />

        <tr v-if="user.isExpanded" v-for="subordinate in user.subordinates" :key="subordinate.id">
          <td>{{ subordinate.name }}</td>
          <td>{{ subordinate.phone }}</td>
        </tr>
      </tbody>
      <tbody v-else>
      <tr>
        <td colspan="2">Нет данных о пользователях.</td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import UserRow from './UserRow';

export default {
  components: { UserRow },
  props: {
    users: {
      type: Array,
    },
  },
  data() {
    return {
      showModal: false,
    };
  },
  methods: {
    sortBy(key) {
      this.users.sort((a, b) => a[key].localeCompare(b[key]));
    },
  },
};
</script>

<style scoped>
.app-left {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  align-self: flex-start;

  width: 45%;
}

table {
  border-collapse: collapse;
  border: 2px solid grey;
  width: 100%;

  color: black;
}

thead {
  border-bottom: 2px solid grey;
}

th,
td {
  border: 1px solid grey;
  padding: 1rem;
  text-align: left;
}

.sort-by-key {
  cursor: pointer;
}
</style>
