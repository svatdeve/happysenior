<template>
  <div class="wrapper">
    <div class="wrapper__inner">
      <div class="wrapper__inner-content">
        <div class="header-logo">
          <div class="header-logo__container">
            <div class="header-logo__logo ibg">
              <img alt="logo" src="../assets/logo.svg" />

            </div>
          </div>
        </div>
        <main class="page">
          <section class="authorization authorization-main">
            <div class="authorization-main__container">
              <div class="authorization-main__wrapper">
                <div class="authorization-main__body">
                  <div class="authorization__title">Welcome back!</div>
                  <div class="authorization__subtitle">Enter your credentials to access your account</div>
                  <Form name="#" class="authorization__form form-authorization" @submit="getAuth">
                    <label :for="emailFieldName">
                      <div class="form-authorization__label">Email</div>
                      <Field :rules="validateEmail" type="text" class="form-authorization__input" :class="{'form-authorization__input_error': isEmailInvalided}" :id="emailFieldName" name="email" placeholder="name@company.com" autocomplete="off" v-model="email" />
                      <div class="form-authorization__error-text" v-if="isEmailInvalided">Invalid email format</div>
                    </label>
                    <label :for="passwordFieldName">
                      <div class="form-authorization__label">Password</div>
                      <Field :rules="validatePassword" type="text" class="form-authorization__input" :class="{'form-authorization__input_error': isPasswordInvalided}" :id="passwordFieldName" name="password" placeholder="Input your password" autocomplete="off" v-model="password" />
                      <div class="form-authorization__error-text" v-if="isPasswordInvalided">This field is 8 symbols min</div>
                    </label>
                    <!--No-task-logic-->
                    <a href="/forgot.html" class="form-authorization__link">Forgot password?</a>
                    <button type="submit" class="form-authorization__button">Sign in</button>
                  </Form>
                </div>
                <div class="authorization__image">
                  <img src="../assets/main-image.svg" alt="men">
                </div>

                <div class="authorization__dots authorization__dots_1">
                  <img src="../assets/dots.svg" alt="dots">
                </div>
                <div class="authorization__dots authorization__dots_2">
                  <img src="../assets/dots.svg" alt="dots">
                </div>
              </div>
            </div>
          </section>
        </main>
      </div>
    </div>
  </div>

</template>

<script setup>
import axios from 'axios'
import { Form, Field, ErrorMessage } from 'vee-validate';
import {ref} from "vue";
const email = ref('support@corecruiter.org')
const password = ref('')
const emailFieldName = ref('authorization-email')
const passwordFieldName = ref('authorization-password')
const isEmailInvalided = ref(false)
const isPasswordInvalided = ref(false)
const getAuth = async (values) => {
  console.log(JSON.stringify(values, null, 2));

  const response = await axios.post('https://api.corecruiter.org/api/user/auth', {"email": email.value, "password": password.value})
  if(response && response.data && response.data.access_token) {
    const token = response.data.access_token
    localStorage.setItem('accessToken', token);
    console.log('Token: ', localStorage.getItem('accessToken'))

  }

}
const validateEmail = (value) => {
  // if the field is empty
  if (!value) {
    isEmailInvalided.value = true
    return 'This field is required';
  }
  // if the field is not a valid email
  const regex = /^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i;
  if (!regex.test(value)) {
    isEmailInvalided.value = true
    return 'Invalid email format';
  }
  // All is good
  isEmailInvalided.value = false

  return true;
}

const validatePassword = (value) => {
  // if the field is empty
  if (!value) {
    isPasswordInvalided.value = true
    return 'This field is required';
  }
  // if the field is not a valid email

  if (value.length < 8) {
    isPasswordInvalided.value = true
    return 'This field is 8 symbols min';
  }
  // All is good
  isPasswordInvalided.value = false

  return true;
}

</script>

<style >

</style>