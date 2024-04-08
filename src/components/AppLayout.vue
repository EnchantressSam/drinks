<script setup>

import { useRoute, useRouter } from "vue-router";
import { ROUTES_PATHS} from '../constants'
import { Back } from "@element-plus/icons-vue";
import { computed } from "vue";

const props = defineProps({
  imgUrl: {
    type: String,
    required: true,
  },
  backFunc: {
    type: Function,
  
  },
  isBackButtonVisible: {
    type: Boolean,
    default: true,
  },
});

const route = useRoute()
const router = useRouter()

const routName = computed(() => route.name)

function goForCocktailRandom() {
  router.push(ROUTES_PATHS.COCKTAIL_RANDOM)
  if(routName.value === ROUTES_PATHS.COCKTAIL_RANDOM) {
    router.go()
  }

}

function goBack() {
  props.backFunc ? props.backFunc() : router.go(-1)

}
</script>

<template>
  <div class="root">
    <div :style="`background-image: url(${imgUrl})`" class="img"></div>
    <div class="main">
      <div class="btns">
        <el-button
          v-if="isBackButtonVisible"
          type="primary"
          :icon="Back"
          circle
          class="back"
          @click="goBack"
        />
        <el-button class="btn" @click="goForCocktailRandom">Get random cocktail</el-button>
      </div>

      <slot></slot>
    </div>
  </div>
</template>

<style lang="sass" scoped>
@import '../assets/styles/main'
.root
    display: flex
    min-height: 100vh
    background-color: $background

.img
    width: 50%
    background-repeat: no-repeat
    background-position: 50% 50%
    background-size: cover
    background-color: rgba(17, 100, 102, 0.4)
    background-blend-mode: overlay

.main
    position: relative
    width: 50%
    padding: 32px 40px

.btn
    position: absolute
    top: 32px
    right: 40px
    font-size: 16px
    font-family: "Raleway", "Arial", sans-serif
    background-color: $accent
    border-color: $accent
    color: $background1

    &:hover,
    &:active
        background-color: darken($text, 10%)
        border-color: darken($text, 10%)
        color: $background

.btns
  display: flex
  justify-content: space-between
  align-items: center

.back
  background-color: transparent
  border-color: $text

  &:hover
    background-color: $accent_hover
    border-color: $accent
</style>
