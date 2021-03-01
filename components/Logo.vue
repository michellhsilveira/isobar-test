<template>
  <nuxt-link to="/">
    <img class="IsobarLogo" src="~/assets/logo.png" alt="Isobar.fm">
  </nuxt-link>
</template>

<style>
.IsobarLogo {
  animation: 1s appear;
  margin: auto;
}

@keyframes appear {
  0% {
    opacity: 0;
  }
}
</style>
