<template>
  <div v-show="showForm" class="app-right">
    <span class="close" @click="handleCLose">&times;</span>
    <!-- Форма для добавления пользователя -->
    <h2 class="form-title">Добавление пользователя</h2>
    <form @submit.prevent="saveUser" class="form">
      <!--       Имя        -->
      <div class="form-group">
        <label for="name">Имя</label>
        <input type="text" id="name" v-model="newUser.name" required>
      </div>
      <!--     Телефон      -->
      <div class="form-group">
        <label for="phone">Телефон</label>
        <input type="text" id="phone" v-model="newUser.phone" required>
      </div>
      <!--     Начальник    -->
      <div class="form-group">
        <label for="phone">Начальник</label>
        <select id="manager" v-model="newUser.manager">
          <option value="" selected></option>
          <option v-for="user in users" :value="user.name" :key="user.id">
            {{ user.name }}
          </option>
        </select>
      </div>
      <!--     Сохранить    -->
      <div class="form-group">
        <button type="submit" class="save-btn">Сохранить</button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  props: {
    showForm: {
      type: Boolean,
      required: true,
      default: false,
    },
    users: {
      type: Array,
      default: this.users,
    },
  },
  data() {
    return {
      newUser: {
        name: '',
        phone: '',
        manager: '',
      },
    };
  },
  methods: {
    handleCLose() {
      this.$emit('onClose');
    },
    saveUser() {
      this.newUser.id = Date.now();
      const userCopy = { ...this.newUser };
      if (this.newUser.manager !== '') {
        const manager = this.users.find(u => u.name === this.newUser.manager);
        if (manager) {
          if (!manager.subordinates) {
            manager.subordinates = [];
          }
          manager.subordinates.push(userCopy);
        }
      } else {
        this.users.push(userCopy);
      }
      localStorage.setItem('users', JSON.stringify(this.users));
      this.newUser = {
        name: '',
        phone: '',
        manager: '',
      };
      this.handleCLose();
    },
  },
};
</script>

<style scoped>
.app-right {
  position: relative;
  display: flex;
  flex-direction: column;
  border: 2px solid grey;
  padding: 1.5rem;
  width: 45%;
  height: 100%;
  color: black;
}

.form-title {
  margin: 0 0 2rem;
  width: 100%;
  text-align: left;
}

.form {
  display: flex;
  flex-direction: column;
  height: 100%;
}

.form-group {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  flex-grow: 1;
  align-items: baseline;
  justify-content: space-between;
}

.form-group:not(:last-child) {
  margin-bottom: 1.5rem;
}

.form-group input,
.form-group select {
  border-radius: 5px;
  border: 2px solid grey;
  width: 70%;
  outline: none;
}

.save-btn {
  border-radius: 20px;
  border: 2px solid grey;

  width: 100px;
  height: 40px;

  font-weight: 600;
  color: black;
  cursor: pointer;
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
