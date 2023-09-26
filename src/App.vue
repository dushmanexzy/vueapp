<template>
  <div id="app">
    <div class="app-container">
      <!--   Add Button   -->
      <button
        @click="openModal"
        class="add-btn"
      >
        Добавить
      </button>
      <div class="app-main">
        <UserTable
          :users="users"
        />
        <UserForm
          :users="users"
          :showForm="showModal"
          @onClose="closeModal"
        />
      </div>
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

button {
  margin: 0;
  padding: 0;
  font-size: inherit;
  color: inherit;
  cursor: pointer;
}

.app-container {
  display: flex;
  flex-direction: column;

  padding: 1rem;
  margin: 0 auto;
  width: 90%;
  height: 100vh;

  font-family: 'Arial', sans-serif;
  font-weight: 400;
  color: grey;
}

.app-main {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  align-items: flex-start;
  justify-content: space-between;
}

.add-btn {
  border-radius: 50px;
  border: 2px solid grey;
  margin: 0 0 2rem calc(45% - 200px);

  width: 200px;
  height: 50px;

  font-weight: 600;
  color: black;
}
</style>
