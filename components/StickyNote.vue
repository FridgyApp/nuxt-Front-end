<template>
  <v-container fluid>
    <v-row>
      <v-col cols="6" v-for="note in listNotes" :key="note._id">
        <v-card class="mx-auto mx-4" max-width="200" min-width="200">
          <v-card-title>{{note.name}}<v-spacer></v-spacer><v-icon>mdi-pencil</v-icon></v-card-title>

          <v-card-text class="text--primary">
            <div>{{note.description}}</div>
          </v-card-text>

          <v-card-actions>
            
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'StickyNote',
  props: {
    notes: Array,
  },
  computed: {
    listNotes() {
      return this.notes
    },
  },
}
</script>