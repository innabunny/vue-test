<template>
  <section>
    <h1>Зарегистрироваться</h1>
    <form @submit.prevent="handleSubmit">
      <div>
        <label for="firstName">
          Имя
          <input
            type="text"
            id="firstName"
            v-model.trim="formUser.firstName"
            required
            @input="validateFirstName"
          />
          <span v-if="errors.firstName" class="error"
            >{{ errors.firstName }}
          </span>
        </label>
      </div>
      <div>
        <label for="lastName">
          Фамилия
          <input
            id="lastName"
            v-model="formUser.lastName"
            type="text"
            @input="validateLastName"
          />
        </label>
        <span v-if="errors.lastName" class="error">{{ errors.lastName }} </span>
      </div>
      <div>
        <label for="phone">
          Телефон
          <input
            id="phone"
            v-model="formUser.phone"
            type="text"
            @input="validatePhone"
          />
        </label>
        <span v-if="errors.phone" class="error">{{ errors.phone }} </span>
      </div>
      <div>
        <label for="email">
          E-mail
          <input
            id="email"
            v-model="formUser.email"
            type="text"
            @input="validateEmail"
          />
          <span v-if="errors.email" class="error">{{ errors.email }} </span>
        </label>
      </div>
      <div>
        <label for="password">
          Пароль
          <input
            id="password"
            v-model="formUser.password"
            type="text"
            @input="validatePassword"
          />
          <span v-if="errors.password" class="error"
            >{{ errors.password }}
          </span>
        </label>
      </div>
      <div>
        <label for="repeatPassword">
          Повторите пароль
          <input
            id="repeatPassword"
            v-model="formUser.repeatPassword"
            type="text"
            @input="validateRepeatPassword"
          />
          <span v-if="errors.repeatPassword" class="error"
            >{{ errors.repeatPassword }}
          </span>
        </label>
      </div>
      <button :disabled="!isFormValid">Отправить</button>
    </form>
    <div v-if="responseMessage" class="response-message">
      {{ responseMessage }}
    </div>
  </section>
</template>

<script lang="ts">
import { computed, defineComponent, reactive, ref } from 'vue';

interface FormData {
  firstName: string;
  lastName: string;
  phone: string;
  email: string;
  password: string;
  repeatPassword: string;
}

interface Errors {
  firstName?: string;
  lastName?: string;
  phone?: string;
  email?: string;
  password?: string;
  repeatPassword?: string;
}

export default defineComponent({
  name: 'FormUser',
  setup() {
    const formUser = ref<FormData>({
      firstName: '',
      lastName: '',
      phone: '',
      email: '',
      password: '',
      repeatPassword: '',
    });

    const errors = reactive<Errors>({});
    const responseMessage = ref<string>('');

    const validateFirstName = () => {
      if (formUser.value.firstName.length < 3) {
        errors.firstName = 'Имя  должно содержать больше 3 символов';
      } else if (formUser.value.firstName.length > 30) {
        errors.firstName = 'Имя должно содержать меньше 30 символов';
      } else {
        console.log('err');
        delete errors.firstName;
      }
    };

    const validateLastName = () => {
      if (formUser.value.lastName.length < 3) {
        errors.lastName = 'Фамилия должна содержать больше 3 символов';
      } else if (formUser.value.lastName.length > 30) {
        errors.lastName = 'Фамилия должна содержать меньше 30 символов';
      } else {
        delete errors.lastName;
      }
    };

    const validatePhone = () => {
      const phoneRegex = /^[0-9]+$/;
      if (!phoneRegex.test(formUser.value.phone)) {
        errors.phone = 'Номер телефона должен содержать только цифры';
      } else {
        delete errors.phone;
      }
    };

    const validateEmail = () => {
      const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      if (!emailPattern.test(formUser.value.email)) {
        errors.email = 'Введите корректный email';
      } else {
        delete errors.email;
      }
    };

    const validatePassword = () => {
      if (formUser.value.password.length < 6) {
        errors.password = 'Пароль должен содержать не менее 6 символов';
      } else {
        delete errors.password;
      }
    };

    const validateRepeatPassword = () => {
      if (formUser.value.password !== formUser.value.repeatPassword) {
        errors.repeatPassword = 'Пароли не совпадают';
      } else {
        delete errors.repeatPassword;
      }
    };

    const isFormValid = computed(() => {
      return Object.keys(errors).length === 0;
    });

    const handleSubmit = () => {
      setTimeout(() => {
        responseMessage.value = 'Регистрация прошла успешно!';
      }, 2000);
    };

    return {
      formUser,
      handleSubmit,
      validateFirstName,
      validateLastName,
      validatePassword,
      validatePhone,
      validateEmail,
      validateRepeatPassword,
      errors,
      isFormValid,
      responseMessage,
    };
  },
});
</script>

<style scoped>
section {
  width: 40vw;
  margin: 4vh;
  padding: 2vw;
  border: 1px solid #ccc;
  border-radius: 5px;
  display: flex;
  flex-direction: column;
  align-items: center;
  box-shadow: 1px 1px lightgray, 1px 1px lightgray;
}

h1 {
  margin-bottom: 1vh;
  font-size: 1.5rem;
}
form {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 60%;
}

form div {
  margin-bottom: 15px;
  width: 100%;
}

label {
  display: block;
  margin-bottom: 5px;
  width: 100%;
}

input {
  width: 100%;
  padding: 8px;
  box-sizing: border-box;
}

button {
  width: 100%;
  padding: 10px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #369f6e;
}

button:disabled {
  cursor: not-allowed;
  opacity: 0.5;
}

.error {
  color: red;
  font-size: 12px;
  padding: 0;
  padding-top: 4%;
  margin: 0;
  line-height: 1%;
}

.response-message {
  margin-top: 15px;
  color: green;
  font-size: 1em;
}
</style>
