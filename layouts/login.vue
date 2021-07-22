<template>
  <v-app>
    <v-main class="bg-color">
      <div class="container-grid">
        <div class="bg-img"></div>
        <div class="form-auth">
          
          <Nuxt />
        </div>
      </div>
    </v-main>
  </v-app>
</template>

<style scoped>

.bg-color {
  background-color: #333;
  background-size: cover;
}
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


.form-auth {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
</style>