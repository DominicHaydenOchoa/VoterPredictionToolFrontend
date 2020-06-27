<template>
  <v-app id="login">
    <v-main>
      <v-container
        class="fill-height"
        fluid
      >
        <v-row
          align="center"
          justify="center"
        >
          <v-col
            cols="12"
            sm="8"
            md="4"
          >
            <v-card class="elevation-12">
              <v-toolbar
                color="blue-grey darken-4"
                dark
                flat
              >
                <v-toolbar-title>Login</v-toolbar-title>
                <v-spacer></v-spacer>
              </v-toolbar>
              <v-card-text>
                <v-form>
                  <v-text-field
                    label="Login"
                    name="login"
                    prepend-icon="mdi-account"
                    type="text"
                    v-model="username"
                  ></v-text-field>

                  <v-text-field
                    id="password"
                    label="Password"
                    name="password"
                    prepend-icon="mdi-lock"
                    type="password"
                    v-model="password"
                  ></v-text-field>
                </v-form>
              </v-card-text>
              <v-card-actions>
                {{loginResult}}
                <v-spacer></v-spacer>
                <v-btn class="white--text" :loading="loading" color="blue-grey darken-4" v-on:click.native="login">
                  Login</v-btn>
              </v-card-actions>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import axios from 'axios'
import router from '../router'
import Dashboard from './Dashboard'
export default {

  data: function () {
    return {
      router: false,
      loginResult: '',
      loading: false,
      username: '',
      password: ''
    }
  },

  methods: {
    login: function () {
      this.loading = true
      axios.get('https://hevm-backend.herokuapp.com/api/login/', {
        params: {
          username: this.username,
          password: this.password
        }
      })
        .then((response) => {
          console.log(response.statusText)
          if (response.statusText === 'OK') {
            console.log('LOGIN SUCCESS')
            this.loading = false
            router.push({ path: '/dashboard', component: Dashboard })
          } else if (response.status === 204) {
            console.log('LOGIN FAILED')
            this.loginResult = 'Invalid login. Try Again.'
            this.loading = false
          }
        })
    }
  },

  computed: {
    bg () {
      return this.background
        ? 'https://wallpaperaccess.com/full/99810.jpg' : undefined
    }
  }
}
</script>

<style>
 #login {
   background-image: url('https://wallpaperaccess.com/full/99810.jpg');
   background-size: cover;
 }
</style>
