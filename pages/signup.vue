<template>
  <v-container>
    <v-form ref="form" class="post-it" lazy-validation>
      <v-text-field v-model="username" label="Username" required></v-text-field>
      <v-text-field
        v-model="email"
        :rules="emailRules"
        label="E-mail"
        required
      ></v-text-field>
      <v-text-field
        v-model="password"
        :rules="passwordRules"
        label="Password"
        required
      ></v-text-field>
      <v-text-field
        v-model="passwordRepeat"
        label="Confirm Password"
        required
      ></v-text-field>
      <v-btn :disabled="!valid" color="success" class="mr-4" @click="validate">
        Sign Up
      </v-btn>
      <v-btn
        to="/login"
        plain
        :disabled="!valid"
        color="success"
        class="my-1"
        block
        @click="validate"
      >
        Log In
      </v-btn>
    </v-form>
  </v-container>
</template>
<script>
export default {
  layout: 'login',
  data: () => ({
    valid: true,
    username: '',
    password: '',
    passwordRepeat: '',
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
      const validate = this.$refs.form.validate()
      if (validate && this.password === this.passwordRepeat) {
        try {
          await this.$axios.$post(`/api/auth/signup`, {
            email: this.email,
            password: this.password,
            name: this.username,
          })
          const res = await this.$auth.loginWith('local', {
            data: {
              email: this.email,
              password: this.password,
            },
          })
          this.$auth.$storage.setUniversal('userId', res.data.uid)
        } catch (error) {
          console.log('muy mal')
        }
      }
    },
  },
}
</script>

<style scoped>
@import url(https://fonts.googleapis.com/css?family=Permanent+Marker);
.post-it {
  width: 400px;
  height: auto;
  position: relative;
  background: #ffa;
  overflow: hidden;
  margin: 150px auto;
  padding: 20px;
  border-radius: 0 0 0 30px/45px;
  box-shadow: inset 0 -40px 40px rgba(0, 0, 0, 0.2),
    inset 0 25px 10px rgba(0, 0, 0, 0.2), 0 5px 6px 5px rgba(0, 0, 0, 0.2);
  font-family: 'Permanent Marker', cursive;
  line-height: 1.7em;
  font-size: 19px;
  -webkit-mask-image: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAIAAACQd1PeAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAA5JREFUeNpiYGBgAAgwAAAEAAGbA+oJAAAAAElFTkSuQmCC);
}
</style>