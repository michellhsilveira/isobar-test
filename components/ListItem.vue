<template>
  <nuxt-link :to="{ name: 'bandas-id', params: { bandas: band.id} }">
    <div class="w-full flex px-8 items-center hover:bg-gray-200 rounded">
      <div class="w-16 flex-shrink-0 h-16 rounded-full overflow-hidden">
        <img class="w-full h-full object-cover" :src="band.image" :alt="band.name">
      </div>
      <div class="flex flex-col ml-4 my-4">
        <span class="text-lg font-medium">{{ band.name }}</span>
        <span class="text-gray-600">{{ band.numPlays | formatPlays }} PLAYS</span>
      </div>
    </div>
  </nuxt-link>
</template>
<script>
export default {
  name: 'ListItem',
  props: {
    band: {}
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
