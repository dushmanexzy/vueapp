<template>
  <div id="app">
    <div class="app-container">
      <button @click="openModal">Добавить пользователя</button>

      <table>
        <thead>
          <tr>
            <th>Имя</th>
            <th>Телефон</th>
          </tr>
        </thead>
        <tbody v-if="users.length > 0">
          <tr v-for="user in users" :key="user.id">
            <td>{{ user.name }}</td>
            <td>{{ user.phone }}</td>
          </tr>
        </tbody>
        <tbody v-else>
          <tr>
            <td colspan="2">Нет данных о пользователях.</td>
          </tr>
        </tbody>
      </table>
      <!-- Модальное окно -->
      <div v-show="showModal" class="modal">
        <div class="modal-content">
          <span class="close" @click="closeModal">&times;</span>
          <!-- Форма для добавления пользователя -->
          <h2>Добавление пользователя</h2>
          <form @submit.prevent="saveUser">
            <!--      Имя      -->
            <div class="form-group">
              <label for="name">Имя</label>
              <input type="text" id="name" v-model="newUser.name" required>
            </div>
            <!--      Телефон      -->
            <div class="form-group">
              <label for="phone">Телефон</label>
              <input type="text" id="phone" v-model="newUser.phone" required>
            </div>
            <!--      Начальник      -->
            <div class="form-group">
              <label for="phone">Начальник</label>
              <select id="manager" v-model="newUser.manager">
                <option value="" selected></option>
                <option v-for="user in users" :value="user.name" :key="user.id">
                  {{ user.name }}
                </option>
              </select>
            </div>
            <div class="form-group">
              <button type="submit">Сохранить</button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
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
  methods: {
    openModal() {
      this.showModal = true;
    },
    closeModal() {
      this.showModal = false;
    },
    saveUser() {
      this.users.push(this.newUser);
      localStorage.setItem('users', JSON.stringify(this.users));
      this.newUser = {
        name: '',
        phone: '',
        manager: '',
      };
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

.modal {
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgba(0, 0, 0, 0.7);
}

.modal-content {
  position: relative;
  background-color: #fff;
  margin: 15% auto;
  padding: 20px;
  border: 1px solid #888;
  width: 50%;
}

.close {
  position: absolute;
  top: 0;
  right: 0;
  padding: 10px;
  font-size: 2rem;
  cursor: pointer;
}
</style>
