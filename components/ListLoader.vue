<template>
  <div class="px-8 max-w-sm w-full mx-auto my-2">
    <div class="animate-pulse flex space-x-4">
      <div class="rounded-full bg-gray-300 h-16 w-16"></div>
      <div class="flex-1 space-y-4 py-1">
        <div class="h-4 bg-gray-300 rounded w-3/4"></div>
        <div class="space-y-2">
          <div class="h-4 bg-gray-300 rounded"></div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'ListLoader'
}
</script>
