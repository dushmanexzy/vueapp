<template>
  <!--   Users Table   -->
  <table>
    <!--    Head Table    -->
    <thead>
    <tr>
      <th @click="sortBy('name')" class="sort-by-key">Имя</th>
      <th @click="sortBy('phone')" class="sort-by-key">Телефон</th>
    </tr>
    </thead>
    <!--    Body Table    -->
    <tbody v-if="users.length > 0">
    <tr v-for="user in users" :key="user.id">
      <td>
        <span class="expand" @click="toggleSubordinates(user)">+</span>
        {{ user.name }}
      </td>
      <td>{{ user.phone }}</td>
    </tr>
    <tr v-if="user.isExpanded" v-for="user in users" :key="user.id">
      <td v-for="subordinate in user.subordinates" :key="subordinate.phone">
        <span>&nbsp;&nbsp;&nbsp;{{ subordinate.name }}</span>
      </td>
      <td v-if="user.isExpanded && user.subordinates">{{ user.phone }}</td>
    </tr>
    </tbody>
    <tbody v-else>
    <tr>
      <td colspan="2">Нет данных о пользователях.</td>
    </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  props: {
    users: {
      type: Array,
      data() {
        const storedUsers = JSON.parse(localStorage.getItem('users')) || [];
        const usersWithDefaults = storedUsers.map(user => ({
          name: user.name || 'Неизвестно',
          phone: user.phone || 'Неизвестно',
        }));
        return {
          showModal: false,
          users: usersWithDefaults,
          newUser: {
            name: '',
            phone: '',
            manager: '',
          },
        };
      },
    },
  },
  methods: {
    sortBy(key) {
      this.users.sort((a, b) => a[key].localeCompare(b[key]));
    },
    toggleSubordinates(user) {
      if (user.subordinates && user.isExpanded) {
        const userElem = user;
        userElem.isExpanded = !userElem.isExpanded;
      }
    },
  },
};
</script>

<style scoped>
.sort-by-key,
.expand {
  cursor: pointer;
}
</style>
