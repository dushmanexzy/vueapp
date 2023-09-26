<template>
  <div id="app">
    <div class="app-container">
      <!--   Add Button   -->
      <button @click="openModal">Добавить пользователя</button>
      <UserTable :users="users"/>
      <UserForm :users="users" :showModal="showModal" @onClose="closeModal"/>
    </div>
  </div>
</template>

<script>
import UserTable from './components/UserTable';
import UserForm from './components/UserForm';

export default {
  name: 'App',
  components: { UserForm, UserTable },
  data() {
    const storedUsers = JSON.parse(localStorage.getItem('users')) || [];
    const usersWithDefaults = storedUsers.map(user => ({
      name: user.name || 'Неизвестно',
      phone: user.phone || 'Неизвестно',
    }));
    return {
      showModal: false,
      users: usersWithDefaults,
    };
  },
  methods: {
    openModal() {
      this.showModal = true;
    },
    closeModal() {
      this.showModal = false;
    },
  },
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
