<template>
  <v-dialog 
    v-model="dialog" 
    persistent 
    max-width="600px"
    transition="fab-transition">
    <template #activator="{ on, attrs }">
      <v-btn 
        block
        x-large 
        color="#FFBA01" 
        light
        v-bind="attrs" v-on="on">
         Add Sticky Note<v-icon dark>mdi-plus</v-icon>
        
      </v-btn>
    </template>
    <v-card>
      <v-card-title>
        <span class="text-h5">Add Sticky Note</span>
      </v-card-title>
      <v-card-text>
        <v-container>
          <v-row>
            <v-col cols="12">
              <v-text-field
                v-model="name"
                label="Name*"
                required
              ></v-text-field>
            </v-col>
            <v-col cols="12">
              <v-text-field
                v-model="description"
                label="Description*"
                required
              ></v-text-field>
            </v-col>
          </v-row>
        </v-container>
        <small>*indicates required field</small>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="#666" text @click="dialog = false">
          Close
        </v-btn>
        <v-btn color="#666" text @click="addPostIt"> Save </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  data: () => ({
    dialog: false,
    name: '',
    description: '',
  }),
  methods: {
    addPostIt() {
      this.$emit('addPostIt', {
        name: this.name,
        description: this.description,
      })
      this.dialog = false
      this.name = ''
      this.description = ''
    },
  },
}
</script>
