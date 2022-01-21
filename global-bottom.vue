<template>
  <footer
    v-if="$slidev.nav.currentPage !== 1"
    class="absolute bottom-0 right-0 p-2"
  >
    {{ $slidev.nav.currentPage }}
  </footer>
</template>
