<template>
  <v-dialog v-model="dialog" persistent max-width="600px" transition="fab-transition">
    <template #activator="{ on, attrs }">
      
      <v-btn class="rounded-pill" color="#333" dark v-bind="attrs" large v-on="on" >
        <v-icon>mdi-plus</v-icon>
        Add Event
      </v-btn>
   
    </template>
    <v-card>
      <v-card-title>
        <span class="text-h5">Add Event</span>
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
              <v-text-field
                v-model="description"
                label="Description*"
                required
              ></v-text-field>
            </v-col>
            <v-col cols="6">
              <input v-model="start" type="datetime-local"  />
            </v-col>
            <v-col cols="6">
              <input v-model="end" type="datetime-local"  />
            </v-col>
          </v-row>
        </v-container>
        <small>*indicates required field</small>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="#F44336" text @click="dialog = false">
          Close
        </v-btn>
        <v-btn color="#4CAF50" text @click="addEvent"> Save </v-btn>
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
    color:'',
    start: '',
    end: '',
    timed: true
  }),
  methods: {
    addEvent() {
      this.$emit('addEvent',{
        name:this.name,
        description:this.description,
        start:this.start,
        end: this.end,
        timed: this.timed
        })
      this.dialog = false
    },
  },
}
</script>