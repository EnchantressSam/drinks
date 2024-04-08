<script setup>
import AppLayout from "../components/AppLayout.vue";
import CocktailThumb from "../components/CocktailThumb.vue";
import { useRootStore } from "../stores/root";
import { storeToRefs } from "pinia";

const rootStore = useRootStore();
rootStore.getIngredients();

const { ingredients, ingredient, cocktails } = storeToRefs(rootStore);


function getCocktails() {
    rootStore.getCocktails(rootStore.ingredient)
}

function removeIngredient() {
    rootStore.setIngredient(null)

}

</script>
<template>
  <AppLayout imgUrl="/src/assets/img/7.jpg" :backFunc="removeIngredient" :is-back-button-visible="!!ingredient">
    <div class="wrapper">
      <div v-if="!ingredient || !cocktails" class="info">
        <div class="title">Choose your drink</div>
        <div class="line"></div>
        <div class="select-wrapper">
          <el-select
            v-model="rootStore.ingredient"
            placeholder="Choose main ingredient"
            size="large"
            filterable
            allow-create
            class="select"
            @change="getCocktails"
          >
            <el-option
              v-for="item in ingredients"
              :key="item.strIngredient1"
              :label="item.strIngredient1"
              :value="item.strIngredient1"
            />
          </el-select>
        </div>
        <div class="text">
            Lorem ipsum dolor sit amet consectetur adipisicing elit. Voluptatibus maxime temporibus id porro omnis molestiae? Laboriosam magni est ipsa cumque.
        </div>
        <img src="/src/assets/img/cocktails.png" alt="Cocktails" class="img">
      </div>
      <div v-else class="info">
        <div class="title">Cocktails with {{ ingredient }}</div>
        <div class="line"></div>
        <div class="cocktailsWithIngr">
            <CocktailThumb v-for="cocktail in cocktails"  :key="cocktail.idDrink" :cocktail="cocktail"/>
        </div>
      </div>
    </div>
  </AppLayout>
</template>

<style lang="sass" scoped>
@import '../assets/styles/main'

.wrapper
    display: flex
    align-items: center
    justify-content: center
.info
    padding: 80px 0
    text-align: center

.select-wrapper
    padding-top: 50px

.select
    width: 220px

.text
    max-width: 516px
    margin: 0 auto
    padding-top: 50px
    line-height: 36px
    letter-spacing: 0.1em
    color: $textMuted

.img
    margin-top: 60px
    max-width: 100%

.cocktailsWithIngr
    display: flex
    align-items: center
    flex-wrap: wrap
    max-height: 400px
    overflow-y: auto
    margin-top: 60px
</style>
