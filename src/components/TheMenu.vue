<template lang="pug">
.menu-container
  .menu-holder
    .menu-subholder.menu-left
      a.menu MENU
        .sidebar-container
          .menu-sidebar
            .menu-item.with-submenu SPECTACLES
              svg(width="11" height="16" viewBox="0 0 11 16" fill="none" class="sidemenu-tab-icon")
                path(d="M1 1L9 8.2L1 15.4" stroke="black" stroke-opacity="0.85" stroke-width="1.5")

              .menu-sidebar-sub
                .menu-item.sub-item(@click="goToUrl('/spectacles-women')")
                  | WOMEN
                .menu-item.sub-item(@click="goToUrl('/spectacles-men')")
                  |  MEN

            .menu-item.with-submenu SUNGLASSES
              svg(width="11" height="16" viewBox="0 0 11 16" fill="none" class="sidemenu-tab-icon")
                path(d="M1 1L9 8.2L1 15.4" stroke="black" stroke-opacity="0.85" stroke-width="1.5")

              .menu-sidebar-sub
                .menu-item.sub-item(@click="goToUrl('/sunglasses-women')")
                  | WOMEN
                .menu-item.sub-item(@click="goToUrl('/sunglasses-men')")
                  | MEN

            .menu-item HOME TRY ON
            .menu-item PAIR FOR PAIR
          
          .sidebar-footer
            .menu-item LOGIN
            .menu-item HELP

      a.visibility FREE EYE TEST
      a.visibility ABOUT US

    .title(@click="goToUrl('/')")
      h2 BLOOBLOOM

    .menu-subholder.menu-right
      a.visibility HELP
      a.visibility LOGIN
      a BAG (0)
    
</template>
<script setup>
function goToUrl(url){
  document.location.replace(url)
}
</script>
<style lang="sass" scoped>
.title
  cursor: pointer

.menu:hover > .sidebar-container
  left: 0

.sidebar-footer
  display: flex
  flex-direction: row
  width: 100%
  border-top: 1px solid black
  & .menu-item
    justify-content: center
    width: 100%
    &:first-child 
      border-right: 1px solid black

.sidebar-container
  position: fixed
  display: flex
  flex-direction: column
  justify-content: space-between
  width: 483px
  z-index: 10
  background: white
  border-right: 1px solid black
  left: -484px
  transition: all 400ms ease
  height: calc(100% - 42px)
  top: 42px
  @media (min-width: 1366px)
    top: 52px
    height: calc(100% - 52px)

.menu-sidebar
  display: flex
  flex-direction: column
  width: 483px

.menu-item:hover
  background: black
  color: white
  & .sub-item
    background: white
    color: black
    &:hover
      background: black
      color: white

.menu-sidebar-sub
  display: none
  position: absolute
  border-right: 1px solid black
  height: 100%
  width: 0px
  overflow: hidden
  top: 0
  left: 484px
  background: white
  z-index: -1
  transition: all 400ms ease

.menu-item
  display: flex
  height: 60px
  justify-content: space-between
  align-items: center
  padding: 0 10px
  border-bottom: 1px solid black
  cursor: pointer

.with-submenu:hover > .menu-sidebar-sub
  width: 481px
  overflow: visible
  display: block

.menu-holder
  display: flex
  justify-content: space-between
  border: 1px solid black
  font-size: 12px
  letter-spacing: 2px
  height: 40px
  @media (min-width: 1366px)
    height: 50px

  .menu-subholder
    display: flex
    justify-content: center
    align-items: center
    a
      cursor: pointer
      display: flex
      justify-content: center
      align-items: center
      height: 100%
      width: 90px
      @media (min-width: 500px) and (max-width: 1365.9px)
        width: 144px
      @media (min-width: 1366px)
        width: 160px
      &:hover
        text-decoration: underline

  .menu-left > a
      border-right: 1px solid black

  .menu-right > a
      border-left: 1px solid black

  .visibility
    display: none !important
    @media (min-width: 1366px)
      display: flex !important
</style>