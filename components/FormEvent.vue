<template>
  <v-dialog v-model="dialog" persistent max-width="600px">
    <template #activator="{ on, attrs }">
      <v-btn class="mx-2" color="primary" dark v-bind="attrs" v-on="on">
        Add Group
      </v-btn>
    </template>
    <v-card>
      <v-card-title>
        <span class="text-h5">ADD EVENT</span>
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
            <v-col cols="12">
              <v-select
                :items="items"
                :menu-props="{ top: true, offsetY: true }"
                label="Label"
              ></v-select>
            </v-col>
            <v-col cols="6">
              <input type="datetime-local" v-model="date" id="" />
            </v-col>
            <v-col cols="6">
              <input type="datetime-local" v-model="date" id="" />
            </v-col>
          </v-row>
        </v-container>
        <small>*indicates required field</small>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="blue darken-1" text @click="dialog = false">
          Close
        </v-btn>
        <v-btn color="blue darken-1" text @click="addEvent"> Save </v-btn>
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
    items: [
      'blue',
      'indigo',
      'deep-purple',
      'cyan',
      'green',
      'orange',
      'grey darken-1',
    ],
    start: '',
    end: '',
    timed: ''
  }),
  methods: {
    addEvent() {
      this.$emit('addEvent')
      this.dialog = false
    },
  },
}
</script>