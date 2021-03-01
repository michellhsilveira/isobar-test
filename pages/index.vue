<template>
  <div>
    <header class="w-full bg-orange-600 h-16 items-center px-4 fixed top-0 left-0 right-0 z-50">
      <div class="container items-center h-full justify-center">
        <div class="relative flex">
          <input v-model="search"
                 class="rounded w-full bg-white h-8 p-4 focus:outline-none placeholder-gray-500 focus:placeholder-black"
                 type="text" name="" id="" placeholder="Pesquisar">
          <span class="absolute right-0 top-0 mr-2 mt-2">
            <img width="16" src="~/assets/search.png" alt="Search">
          </span>
        </div>
        <div>
          <logo class="w-24 flex-shrink-0 ml-4 inline-block"/>
        </div>
      </div>
    </header>

    <div class="container flex-col pt-16">
      <div class="px-8 text-sm text-gray-700 bg-gray-200 flex items-center">
        {{ filteredBands.length }} resultados
        <div class="ml-auto relative h-8 ">
          <button class="w-8 h-8 relative" @click="showFilter = !showFilter">
            <img src="~/assets/order_by.png" alt="Ordenar">
          </button>
          <div class="absolute top-auto right-0 bg-gray-700" v-show="showFilter">
            <span @click="sortData('name','asc')" :class="{
              'font-medium' : sortBy === 'name'
            }" class="w-full p-3 block text-white uppercase whitespace-no-wrap">ORDEM ALFABÉTICA</span>
            <span @click="sortData('numPlays','desc')" :class="{
              'font-medium' : sortBy === 'numPlays'
            }" class="w-full p-3 block text-white uppercase whitespace-no-wrap">POPULARIDADE</span>
          </div>
        </div>
      </div>
      <div class="h-full">
        <div v-if="$fetchState.pending">
          <ListLoader/>
          <ListLoader/>
          <ListLoader/>
        </div>
        <template v-else v-for="band in filteredBands">
          <ListItem v-bind:key="band.id" :band="band"/>
        </template>
        <div class="flex h-screen items-center justify-center flex-col text-gray-700 text-xl" v-if="filteredBands.length === 0 && !$fetchState.pending">
          <span class="mb-4">Sem Resultados...</span>
          <img class="w-40" src="~/assets/no_results.png" alt="Sem resultados">
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import ListItem from "~/components/ListItem";
import ListLoader from "~/components/ListLoader";
import _ from 'lodash'

export default {

  components: {ListLoader, ListItem},
  data() {
    return {
      bands: [],
      showFilter: false,
      sortBy: 'name',
      sortOrder: 'asc',
      search: ''
    }
  },
  methods: {
    sortData(type, order) {
      this.sortBy = type;
      this.sortOrder = order;
      this.showFilter = false;
    }
  },
  computed: {
    filteredBands() {
      return _.orderBy(this.bands.filter(band => {
        return band.name.toLowerCase().indexOf(this.search.toLowerCase()) > -1
      }), [this.sortBy], [this.sortOrder])
    }
  },
  async fetch() {
    this.bands = await fetch(
      'https://iws-brazil-labs-iws-recruiting-bands.iwsbrazil.io/api/bands'
    ).then(res => res.json())
  },
}
</script>

<style>
.container {
  @apply flex mx-auto;
}
</style>
