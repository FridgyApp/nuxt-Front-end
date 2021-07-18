<template>
  <v-app>
    <v-main class="bg-color">
      <div class="container-grid">
        <div class="bg-img"></div>
        <div class="form-auth">
          <h1>Formulario</h1>
          <Nuxt />
        </div>
      </div>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      links: ['Home', 'About Us', 'Contact Us'],
    }
  },
}
</script>

<style scoped>
.container-grid {
  display: grid;
  grid-template-columns: 100%;
  height: 100%;
}

.bg-img{
  display:none;
}

@media (min-width: 700px) {
  .container-grid {
    display: grid;
    grid-template-columns: 35% 65%;
    height: 100%;
  }
  .bg-img {
    display: block;
    background-image: url('@/static/FridgeWallpaper.jpg');
    background-repeat: no-repeat;
    background-position: right;
    background-size: cover;
  }
}
.bg-color {
  background-image: radial-gradient(
    circle at 85.04% -10.69%,
    #fff5d2 0,
    #f2deb5 50%,
    #e0c799 100%
  );
}

.form-auth {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
</style>