<template>
  <v-container>
    <v-container>
    <v-row>
      <v-col class="d-flex justify-center mb-6 container-login">
        <v-form ref="form" v-model="valid" class="post-it" lazy-validation>
          <v-card class="form-login" rounded>
            <v-text-field
              v-model="username" 
              solo
              label="Username" 
              required
             ></v-text-field>
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
            <v-text-field
              v-model="passwordRepeat"
              solo
              label="Confirm Password"
              required
            ></v-text-field>
            <v-btn
              :disabled="!valid"
              color="#FFBA01"
              class="my-1"
              block
              @click="validate"
            >
              Sign Up
            </v-btn>
            <v-btn
             to="/signup" 
             block plain> 
              Exit 
            </v-btn>
          </v-card>
        </v-form>
      </v-col>
    </v-row>
  </v-container>
    
      
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
.my-1 {
  border-radius: 50px;
}
.form-login {
  padding: 2rem;
  background-color: #666;
}
.post-it {
  width: 60%;
}
</style>