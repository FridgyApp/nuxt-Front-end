<template>
  <div>
    <v-container>
      <v-row>
        <div class="logo-login mb-5">
          <img src="../static/logo-blanco.png" />
        </div>
      </v-row>
    </v-container>
    <v-container>
      <v-row>
        <v-col class="d-flex justify-center mb-6 container-login">
          <v-form ref="form" v-model="valid" class="post-it" lazy-validation>
            <v-card class="form-login" rounded>
              <v-text-field
                v-model="email"
                class="rounded-pill"
                solo
                :rules="emailRules"
                label="E-mail"
                required
              ></v-text-field>
              <v-text-field
                v-model="password"
                class="rounded-pill"
                :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
                solo
                :rules="passwordRules"
                :type="show1 ? 'text' : 'password'"
                name="input-10-1"
                label="Password"
                hint="At least 6 characters"
                counter
                required
                @click:append="show1 = !show1"
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
            <v-row v-if="loginError" class="d-flex justify-center">
              <ErrorModal :error-message="errorMessage" />
            </v-row>
          </v-form>
        </v-col>
      </v-row>
    </v-container>
    <v-footer class="footer" color="grey darken-4" padless app>
      <v-row justify="center" no-gutters>
        <v-col class="grey darken-4 py-4 text-center white--text" cols="12">
          {{ new Date().getFullYear() }} — <strong>Fridg.App</strong> — by Bruno
          Aggierni <a href="https://www.linkedin.com/in/bruno-aggierni/"><v-icon color="blue">mdi-linkedin</v-icon
          ></a>, Álvaro Poncio <a href="https://www.linkedin.com/in/alvaro-poncio/"><v-icon color="blue"
            >mdi-linkedin</v-icon
          ></a>, Adrian Duran <a href="https://www.linkedin.com/in/adrian-duran-gomez/"><v-icon color="blue">mdi-linkedin</v-icon
          ></a>
        </v-col>
      </v-row>
    </v-footer>
  </div>
</template>
<script>
export default {
  layout: 'login',
  data: () => ({
    show1: false,
    valid: true,
    password: '',
    loginError: false,
    errorMessage: '',
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
      try {
        this.$refs.form.validate()
        if (this.password !== '' && this.email !== '') {
          await this.$auth.loginWith('local', {
            data: {
              email: this.email,
              password: this.password,
            },
          })
        }
      } catch (error) {
        this.loginError = !this.loginError
        this.errorMessage = error.response.data.msg
        console.clear()
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




