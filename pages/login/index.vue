<template>
  <div class="section-login">
    <v-row class="row-column" justify="center" align="center">
      <v-col cols="12" sm="8" md="4">
        <v-card class="box-login">
          <v-card-title class="headline mb-5">
            Welcome to My Page
          </v-card-title>
          <v-card-text>
            <v-text-field
              v-model="email"
              outlined
              :rules="[rules.required, rules.email]"
              label="E-mail"
            ></v-text-field>
            <v-text-field
              v-model="password"
              outlined
              :append-icon="showPw ? 'mdi-eye' : 'mdi-eye-off'"
              :rules="[rules.required, rules.min]"
              :type="showPw ? 'text' : 'password'"
              name="input-10-1"
              label="Password"
              hint="At least 8 characters"
              @click:append="showPw = !showPw"
            ></v-text-field>
          </v-card-text>
          <v-card-actions class="flex-column">
            <v-btn
              color="info"
              block
              @click="toLogin()"
            >
              Login
            </v-btn>
            <div class="section-or my-2">
              <span class="line"></span>
              <p class="my-0 mx-4">or</p>
              <span class="line"></span>
            </div>
            <v-btn
              class="mx-0"
              depressed
              block
              to="/register"
            >
              register
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
import CallApiLibrary from '@/lib/callApiLibrary'
export default {
  mixins: [ CallApiLibrary ],
  data () {
    return {
      title: 'Login',
      email: '',
      password: '',
      showPw: false,
      rules: {
        required: value => !!value || 'Required.',
        min: v => v.length >= 8 || 'Min 8 characters',
        email: value => {
          const pattern = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
          return pattern.test(value) || 'Invalid e-mail.'
        },
      },
    }
  },
  head () {
    return {
      title: this.title
    }
  },
  methods: {
    toLogin () {
      const postData = {
        email: this.email,
        password: this.password
      }
      this.callApiPost('/api/login', postData, responseData => {
        console.log(responseData)
        this.showNotificationMessage('Login Succes')
        this.$router.push('/')
      }, responseData => {
      }, responseData => {
      }, true, true)
    }
  }
}
</script>
