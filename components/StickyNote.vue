<template>
<v-hover v-slot="{ hover }">
  <v-card class="mx-auto mx-4" max-width="400" min-width="200" min-height="120" >
    <v-card-title
      >{{ note.name }}
      <v-spacer></v-spacer>
      <v-dialog v-model="dialog" persistent max-width="600px" transition="fab-transition">
        <template #activator="{ on, attrs }">
          <v-icon :color="hover?'grey darken-2':'transparent'"  v-bind="attrs" v-on="on" 
            >mdi-square-edit-outline</v-icon
          >
        </template>
        <v-card >
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
            <v-btn color="#666" text @click="editNote"> Save </v-btn>
          </v-card-actions>
        </v-card> 
      </v-dialog>
        <v-icon :color="hover?'grey darken-2':'transparent'" @click="eraseNote(note._id)"
        >mdi-trash-can-outline</v-icon
      ></v-card-title
    >
    <v-card-text class="text--primary">
      <div>{{ note.description }}</div>
    </v-card-text>
  </v-card>
</v-hover>
</template>

<script>
export default {
  data: () => ({
    name: '',
    dialog: false,
    description: '',
  }),
  props: {
    note: Object,
  },
  methods: {
    eraseNote(id) {
      this.$emit('delete', id)
    },
    editNote() {
      this.$emit('edit', this.newNote)
      this.dialog = false
    },
    
  },

  computed: {
    newNote() {
      return {
        _id: this.note._id,
        name: this.name,
        description: this.description,
      }
    },
    
  },
}
</script>