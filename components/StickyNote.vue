<template>
  <v-container fluid>
    <v-row>
      <v-col cols="6" v-for="note in listNotes" :key="note._id">
        <v-card class="mx-auto mx-4" max-width="200">
          <v-card-title>{{note.name}}</v-card-title>

          <v-card-text class="text--primary">
            <div>{{note.description}}</div>
          </v-card-text>

          <v-card-actions>
            <v-btn color="orange" text> Share </v-btn>

            <v-btn color="orange" text> Explore </v-btn>
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