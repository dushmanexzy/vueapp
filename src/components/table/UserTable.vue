<template>
  <div class="app-left">
  <!--   Users Table   -->
    <table class="users-table">
      <!--    Head Table    -->
      <thead>
      <tr>
        <th @click="sortParam='name'" class="sort-by-key">Имя</th>
        <th @click="sortParam='phone'" class="sort-by-key">Телефон</th>
      </tr>
      </thead>
      <!--    Body Table    -->
      <tbody v-if="users.length > 0" v-for="user in sortedList" :key="user.id">
        <UserRow :user="user" :users="users" />

        <tr v-if="user.isExpanded" v-for="subordinate in user.subordinates" :key="subordinate.id">
          <td>{{ subordinate.name }}</td>
          <td>{{ subordinate.phone }}</td>
        </tr>
      </tbody>
      <!--    Body Table Else    -->
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
      sortParam: '',
    };
  },
  computed: {
    sortedList() {
      switch (this.sortParam) {
        case 'name': return this.users.slice().sort(this.sortByName);
        case 'phone': return this.users.slice().sort(this.sortByPhone);
        default: return this.users;
      }
    },
  },
  methods: {
    sortByName(a, b) {
      return (a.name.toLowerCase() > b.name.toLowerCase()) ? 1 : -1;
    },
    sortByPhone(a, b) {
      return (a.phone.toLowerCase() > b.phone.toLowerCase()) ? 1 : -1;
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
