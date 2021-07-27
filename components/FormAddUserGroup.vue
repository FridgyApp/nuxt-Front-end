<template>
    <v-dialog

      v-model="dialog"
      persistent
      max-width="600px"
      transition="fab-transition"
    >
      <template #activator="{ on, attrs }">
        <v-btn
          plain
          v-bind="attrs"
          v-on="on"
        >
          Add User in Group
        </v-btn>
      </template>
      <v-card>
        <v-card-title>
          <span class="text-h5">Add User in Group</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col cols="12">
                <v-text-field
                  v-model="email"
                  label="Email*"
                  required
                ></v-text-field>
              </v-col>
            </v-row>
          </v-container>
          <small>*indicates required field</small>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="#666"
            text
            @click="dialog = false"
          >
            Close
          </v-btn>
          <v-btn
            color="#666"
            text
            @click="addUserGroup"
          >
            Save
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
</template>

<script>
  export default {
    data: () => ({
      dialog: false,
      email:''
    }),
    methods: {
        addUserGroup() {
            this.$emit('addUserGroup', this.email)
            this.dialog = false
            this.email = ''
        }
    },
  }
</script>

<style scoped>
.text-h5 {
  font-family: 'Dosis', sans-serif !important
}
</style>

