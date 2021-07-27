<template>
  <div>
    <v-alert
      border="left"
      color="red"
      dismissible
      elevation="9"
      dense
      type="error"
      >{{errorMessage}}</v-alert
    >
  </div>
</template>

<script>
export default {
  props: ['errorMessage'],
  
}
</script>