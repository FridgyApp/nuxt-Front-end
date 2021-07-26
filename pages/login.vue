<template>
  <div>
    <v-container>
      <v-row>
        <div class="logo-login mb-5" >
          <img src="../static/logo-blanco.png">  
        </div>
      </v-row>
    </v-container>
  <v-container>
    <v-row>
      <v-col class="d-flex justify-center mb-6 container-login">
        <v-form ref="form" v-model="valid" class="post-it" lazy-validation>
          <v-card class="form-login" rounded>
            <v-text-field
              class="rounded-pill"
              v-model="email"
              solo
              :rules="emailRules"
              label="E-mail"
              required
            ></v-text-field>
            <v-text-field
              class="rounded-pill"
              v-model="password"
              solo
              :rules="passwordRules"
              label="Password"
              required
            ></v-text-field>
            <v-btn
              :disabled="!valid"
              color="#FFBA01"
              class="rounded-pill my-1"
              block
              @click="validate"
            >
              Log In
            </v-btn>
            <v-btn to="/signup" block plain color="white"> Sign Up </v-btn>
          </v-card>
        </v-form>
      </v-col>
    </v-row>
  </v-container>
  <v-footer class="footer" color="grey darken-4" padless app>
      <v-row justify="center" no-gutters>
        <v-col class="grey darken-4 py-4 text-center white--text" cols="12">
          {{ new Date().getFullYear() }} — <strong>Fridge.App</strong>
        </v-col>
      </v-row>
    </v-footer>
  </div>
</template>
<script>
export default {
  layout: 'login',
  data: () => ({
    valid: true,
    password: '',
    passwordRules: [
      (v) => !!v || 'Password is required',
      (v) => (v && v.length >= 6) || 'Name must be less than 6 characters',
    ],
    email: '',
    emailRules: [
      (v) => !!v || 'E-mail is required',
      (v) => /.+@.+\..+/.test(v) || 'E-mail must be valid',
    ],
  }),

  methods: {
    async validate() {
      this.$refs.form.validate()
      if (this.password !== '' && this.email !== '') {
        await this.$auth.loginWith('local', {
          data: {
            email: this.email,
            password: this.password,
          },
        })
      }
    },
  },
}
</script>

<style scoped>

.form-login {
  padding: 2rem;
  background-color: #666;
}
.post-it {
  width: 60%;
}
</style>




