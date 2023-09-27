<template>
  <div v-show="showForm" class="app-right">
    <span class="close" @click="handleCLose">&times;</span>
    <!-- Форма для добавления пользователя -->
    <h2 class="form-title">Добавление пользователя</h2>
    <form @submit.prevent="saveUser" class="form">
      <!--       Имя        -->
      <BaseInputText
        v-model="newUser.name"
        :id-name="String('name')"
        :label="String('Имя')"
        class="form-group"
      />
      <!--     Телефон      -->
      <BaseInputText
        v-model="newUser.phone"
        :id-name="String('phone')"
        :label="String('Телефон')"
        class="form-group"
      />
      <!--     Начальник    -->
      <BaseSelectUsers
        v-model="newUser.manager"
        :label="String('Начальник')"
        :id-name="String('manager')"
        :users="users"
        class="form-group"
      />
      <!--     Сохранить    -->
      <div class="form-group">
        <button type="submit" class="save-btn">Сохранить</button>
      </div>
    </form>
  </div>
</template>

<script>
import BaseInputText from './BaseInputText';
import BaseSelectUsers from './BaseSelectUsers';

export default {
  components: { BaseSelectUsers, BaseInputText },
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

.save-btn {
  border-radius: 20px;
  border: 2px solid grey;

  width: 120px;
  height: 45px;

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
