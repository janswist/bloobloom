<script setup>
import TheSite from '@/components/TheSite.vue'
import TheMenu from '@/components/TheMenu.vue'
</script>

<template lang="pug">
header
  TheMenu

main
  TheSite

</template>

<style lang="sass">
@font-face
  font-family: Acumin Pro
  src: url('./fonts/Acumin-Rpro.woff')

body
  margin: 0px

#app
  font-family: 'Acumin Pro', sans-serif
  font-weight: 400
  -webkit-font-smoothing: antialiased 
  -moz-osx-font-smoothing: grayscale 
</style>
