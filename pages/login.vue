<template>
  <v-container>
    <v-row>
      <v-col class="d-flex justify-center mb-6 container-login">
        <v-form ref="form" v-model="valid" class="post-it" lazy-validation>
          <v-card class="form-login" rounded>
            <v-text-field
              v-model="email"
              solo
              :rules="emailRules"
              label="E-mail"
              required
            ></v-text-field>
            <v-text-field
              v-model="password"
              solo
              :rules="passwordRules"
              label="Password"
              required
            ></v-text-field>
            <v-btn
              :disabled="!valid"
              color="#FFBA01"
              class="my-1"
              block
              @click="validate"
            >
              Log In
            </v-btn>
            <v-btn to="/signup" block plain> Sign Up </v-btn>
          </v-card>
        </v-form>
      </v-col>
    </v-row>
  </v-container>
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




