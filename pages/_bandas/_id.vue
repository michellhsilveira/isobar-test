<template>
  <div>
    <header class="w-full bg-orange-600 h-16 bg-opacity-75 items-center px-4 fixed top-0 left-0 right-0 z-50">
      <div class="container items-center h-full justify-center">
        <div class="w-16 mr-auto">
          <nuxt-link to="/">
            <img class="w-12" src="~/assets/chevron_left-black-18dp.svg" alt="Voltar">
          </nuxt-link>
        </div>
        <div>
          <logo class="w-24 flex-shrink-0 ml-4 inline-block"/>
        </div>
        <div class="w-16 ml-auto">
        </div>
      </div>
    </header>
    <div class="container flex-col">
      <div v-if="$fetchState.pending">
        <div class="w-full flex h-64 bg-gray-500 bg-cover bg-center relative mb-24 animate-pulse">
          <div class="mt-auto -mb-20 flex w-full flex-col text-center z-20">
            <div class="space-y-2 w-1/3 mx-auto mb-2">
              <div class="h-4 bg-gray-300 rounded"></div>
            </div>
            <div class="flex items-center px-8">
              <div class="w-1/3">
                <div class="h-4 bg-gray-300 rounded"></div>
              </div>
              <div class="w-1/3">
                <div
                  class="w-24 h-24 flex-shrink-0 animate-pulse bg-gray-300 rounded-full overflow-hidden mx-auto mb-8">
                </div>
              </div>
              <div class="uppercase text-sm text-gray-500 w-1/3 tracking-tight text-right">
                <div class="h-4 bg-gray-300 rounded"></div>
              </div>
            </div>
          </div>
        </div>
        <div class="prose mx-8">
          <div class="space-y-2">
            <div class="h-4 bg-gray-300 rounded"></div>
            <div class="h-4 bg-gray-300 rounded"></div>
          </div>
        </div>
        <button class="text-3xl w-full bg-gray-300 animate-pulse text-gray-300 mt-2">+</button>
        <!--Albuns-->
        <h3 class="bg-gray-300 animate-pulse rounded mx-auto text-gray-300 w-1/3 mb-4 mt-2">Albuns</h3>


        <div class="flex flex-wrap">
          <template v-for="index in 3">
            <div class="w-1/3 flex relative">
              <div class="aspect-w-4 border border-gray-400 aspect-h-4 w-full animate-pulse bg-gray-300"></div>
            </div>
          </template>
        </div>
      </div>

      <div class="flex flex-col" v-else>
        <div class="w-full flex h-64 bg-gray-500 bg-cover bg-center relative mb-24 overlay"
             v-bind:style="{ 'background-image': 'url(' + band.image + ')' }">
          <div class="mt-auto -mb-20 flex w-full flex-col text-center z-20">
            <h2 class="text-2xl text-white">{{ band.name }}</h2>
            <div class="flex items-center px-8">
              <div class="uppercase text-sm text-gray-500 w-1/3 text-left">{{ band.genre }}</div>
              <div class="w-1/3">
                <div class="w-24 h-24 flex-shrink-0  rounded-full overflow-hidden mx-auto mb-8">
                  <img class="w-full h-full object-cover" :src="band.image" :alt="band.name">
                </div>
              </div>
              <div class="uppercase text-sm text-gray-500 w-1/3 tracking-tight text-right">
                {{ band.numPlays | formatPlays }} PLAYS
              </div>
            </div>
          </div>
        </div>
        <div class="prose mx-8" :class="{
          'h-16 overflow-hidden fade-out' : !showDetails
        }" v-html="band.biography"></div>
        <button v-if="!showDetails" @click="showDetails = !showDetails" class="text-3xl">+</button>
        <h3 class="uppercase text-lg text-center mt-4">Albuns</h3>

        <div class="flex flex-wrap">
          <template v-for="album in albuns">
            <nuxt-link class="w-1/3 flex relative" v-bind:key="album.id"
                       :to="{name: 'bandas-album-id', params: {bandas: band.id, id: album.id}}">
              <span class="absolute bottom-0 text-white z-10 text-sm p-2 leading-4 z-10">{{ album.name }}</span>
              <div class="aspect-w-4 aspect-h-4 w-full overlay-xs">
                <img class="w-full h-full object-cover" :src="album.image" :alt="album.name">
              </div>
            </nuxt-link>
          </template>
        </div>
      </div>
    </div>
  </div>
</template>


<script>

export default {
  components: {},
  data() {
    return {
      band: {},
      albuns: [],
      showDetails: false
    }
  },
  async fetch() {
    const band = await fetch(`https://iws-brazil-labs-iws-recruiting-bands.iwsbrazil.io/api/bands/${this.$route.params.bandas}`)
      .then((res) => res.json())

    if (band.id === this.$route.params.bandas) {
      this.band = band;


      const albuns = await fetch(`https://iws-brazil-labs-iws-recruiting-bands.iwsbrazil.io/api/albums`)
        .then((res) => res.json())


      this.albuns = albuns.filter(album => {
        return album.band === band.id;
      });

    } else {
      // set status code on server and
      if (process.server) {
        this.$nuxt.context.res.statusCode = 404
      }
      // use throw new Error()
      throw new Error('Banda não encontrada')
    }
  },
  filters: {
    formatPlays: function (value) {
      if (!value) return ''
      let numberFormat = Intl.NumberFormat();

      return numberFormat.format(value);
    }
  }
}
</script>

<style>
.container {
  @apply flex mx-auto;
}

.fade-out {
  position: relative;
}

.fade-out:before {
  content: '';
  width: 100%;
  height: 100%;
  position: absolute;
  left: 0;
  top: 0;
  background: linear-gradient(0deg, white, transparent);
}

.overlay:before {
  content: '';
  width: 100%;
  height: 100%;
  position: absolute;
  left: 0;
  top: 0;
  background: linear-gradient(0deg, black, transparent);
  z-index: 1;
}

.overlay-xs:before {
  content: '';
  width: 100%;
  height: 30%;
  position: absolute;
  left: 0;
  bottom: 0;
  background: linear-gradient(0deg, black, transparent);
  z-index: 1;
}
</style>
