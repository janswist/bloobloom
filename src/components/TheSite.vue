<template lang="pug">
.collection-header
  div.hide-on-small-devices
  .title
    h1 {{ title }}
    span {{ resultsTotal }} found
  
  .button-box
    .filter-button
      .dropdown
        .filter-name COLOUR
        .dropdown-content.left
          label.wrapper(
            v-for="colour in coloursOptions"
            :for="colour"
          )
            input.sr-only(
              type="checkbox"
              :id="colour"
              :value="colour"
              v-model="coloursChecked"
            )
            span(
              :class="{'checked': coloursChecked.includes(colour)}"
            ) {{  colour }}

    .filter-button
      .dropdown
        .filter-name SHAPE
        .dropdown-content.right
          label.wrapper(
            v-for="shape in shapeOptions"
            :for="shape"
          )
            input.sr-only(
              type="checkbox"
              :id="shape"
              :value="shape"
              v-model="shapesChecked"
            )
            span(
              :class="{'checked': shapesChecked.includes(shape)}"
            ) {{  shape }}

.collection
  .collection-item(v-for="item in collection")
    .item-title
      p {{ item.name }}
    img(:src="item.glass_variants[0].media[0].url")

</template>
<script setup>
import { ref, computed, watch, watchEffect, nextTick } from 'vue'

const baseUrl = 'https://staging-api.bloobloom.com/user/v1/sales_channels/website/collections'

const collection =   ref([])
const resultsPage =  ref(1)
const resultsTotal = ref(0)

const coloursOptions = ref(['black','tortoise','coloured','crystal','dark','bright'])
let coloursChecked =   ref([])

const shapeOptions = ref(['square','rectangle','round','cat-eye'])
let shapesChecked =  ref([])

const title = computed(() => {
  const path = document.location.pathname
  return path === '/'
    ? 'SPECTACLES WOMEN'
    : path
      .replace('/','')
      .replace('-',' ')
      .toUpperCase()
})

watch([coloursChecked, shapesChecked], () => {
  resultsPage.value = 1
})

watchEffect(async () => {
  const collectionPath = document.location.pathname === '/'
    ? '/spectacles-women'
    : document.location.pathname
  
  const colourSelectionToUrl = selectionUrlBuilder('colour', coloursChecked)
  const shapeSelectionToUrl =  selectionUrlBuilder('frame', shapesChecked)

  const targetUrl = `${baseUrl}/${collectionPath}/glasses?`
    + `sort[type]=collection_relations_position`
    + `&sort[order]=asc`
    + `&page[number]=${resultsPage.value}`
    + `&page[limit]=12`
    + `&filters[lens_variant_types][]=classic`
    + `&filters[lens_variant_prescriptions][]=fashion`
    + `&filters[frame_variant_home_trial_available]=false`
    + `${shapeSelectionToUrl}`
    + `${colourSelectionToUrl}`

  const data = await fetchCollection(targetUrl)
  collection.value = [
    ...resultsPage.value > 1 ? collection.value : [],
    ...data
  ]
  
  await nextTick()
  startObserver()
})

function selectionUrlBuilder (tag, checkedFilters){
  return checkedFilters.value.length > 0
    ? checkedFilters.value
        .map(item => `&filters[glass_variant_frame_variant_${tag}_tag_configuration_names][]=${item}`)
        .join('')
    : ''
}

async function fetchCollection(url){
  const { glasses, meta:{ total_count }} = await (await fetch(url)).json()
  
  resultsTotal.value = total_count
  
  return glasses
}

function startObserver() {
  const allItems =      document.querySelectorAll('.collection-item')
  const lastItemIndex = allItems.length - 1
  const lastItem =      allItems[lastItemIndex]

  const options = {
      root: null,
      rootMargin: "0px",
      threshold: 0.1
  }

  const callback = (entries, observer) => {
    entries.forEach((entry) => {
      const isAllResultsLoaded = collection.value.length === resultsTotal.value
      if(entry.isIntersecting && !isAllResultsLoaded){
        resultsPage.value += 1
      }
    })
  }

  const observer = new IntersectionObserver(callback, options)

  observer.observe(lastItem)
}

</script>
<style lang="sass" scoped>
.collection-header
  display: grid
  align-items: flex-start
  text-align: center
  font-size: 13px
  grid-template-columns: 100%
  @media (min-width: 891px) and (max-width: 1318.9px)
    grid-template-columns: 50% 50%
  @media (min-width: 1319px)
    grid-template-columns: 33.33% 33.33% 33.33%

  & div
    width: 100%

  .title
    padding-top: 5px
    border-left: 1px solid black
    border-right: 1px solid black
    width: calc(100% - 1px)
    @media (min-width: 891px) and (max-width: 1318.9px)
      width: calc(100% - 1px)
    @media (min-width: 1319px)
      width: 100%
    
    h1
      margin-bottom: 0

  .button-box
    display: flex
    height: 100%

.collection
  display: grid
  grid-gap: 1px
  background: black
  justify-items: center
  align-items: flex-start
  text-align: left
  color: black
  border-top: 1px solid black
  grid-template-columns: 100%
  @media (min-width: 891px) and (max-width: 1318.9px)
    grid-template-columns: 50% 50%
  @media (min-width: 1319px)
    grid-template-columns: 33.33% 33.33% 33.33%

.collection-item
  display: flex
  position: relative
  img
    width: 100%
    height: 100%
    object-fit: cover
  
  .item-title
    display: flex
    position: absolute
    width: 100%
    height: 40px
    color: black
    justify-content: center
    font-size: 24px

.hide-on-small-devices
  display: none
  @media (min-width: 1319px)
    display: block

.filter-button
  border-top: 1px solid black
  border-left: 1px solid black
  &:not(:first-child)
    border-right: 1px solid black
  cursor: pointer
  min-height: 40px
  @media (min-width: 891px)
    border-top: none
    border-left: none
    border-right: 1px solid black
  
  .filter-name
    display: flex
    width: 100%
    height: 100%
    justify-content: center
    align-items: center

  .dropdown
    position: relative
    display: inline-block
    width: 100%
    height: 100%

    .dropdown-content
      position: absolute
      display: none
      z-index: 1
      border: 1px solid black
      background: white
      &.left
        left: -1px
        width: 100%
        @media (min-width: 891px)
          width: calc(100% - 1px)
          left: 0px
        @media (min-width: 1319px)
          width: calc(100% - 2px)
          left: 1px
      &.right
        left: -1px


      label
        cursor: pointer

      .wrapper
        display: flex
        justify-content: center
        align-items: center
        height: 60px
        width: 100%
        margin: auto
        .checked
          text-decoration: underline

  .dropdown:hover .dropdown-content
    display: block

  .sr-only
    clip: rect(0 0 0 0)
    clip-path: inset(100%)
    height: 1px
    overflow: hidden
    position: absolute
    white-space: nowrap
    width: 1px
</style>