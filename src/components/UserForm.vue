<template>
  <!-- Модальное окно -->
  <div v-show="showModal" class="modal">
    <div class="modal-content">
      <span class="close" @click="handleCLose">&times;</span>
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
          <button @click.prevent="saveUser">Сохранить</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    showModal: {
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
