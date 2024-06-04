<script setup>
import AppLayout from "@/components/AppLayout.vue";
import {useRootStore} from "@/stores/root.js";
import {storeToRefs} from "pinia";
import CocktailThumb from "@/components/CocktailThumb.vue";

const rootStore = useRootStore();
rootStore.getIngredients()

const {ingredients, ingredient, cocktails} = storeToRefs(rootStore);

function getCocktails() {
  rootStore.getCocktails(rootStore.ingredient)
}
</script>

<template>
  <AppLayout
      imgUrl="/src/assets/img/bg-1.jpg"
  >
    <div class="wrapper">
      <div v-if="!ingredient || !cocktails" class="info">
        <div class="title">Choose your drink</div>
        <div class="line"></div>
        <div class="select-wrapper">
          <el-select
              v-model="rootStore.ingredient"
              placeholder="Choose main ingredient"
              size="large"
              style="width: 240px"
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
          Try our delicious cocktail recipes for every occasion. Find delicious cocktail recipes by ingredient through our cocktail generator.
        </div>
        <img src="../assets/img/cocktails.png" alt="Cocktails" class="img">
      </div>
      <div v-else class="info">
        <div class="title">COCKTAILS WITH {{ingredient}} </div>
        <div class="line"></div>
        <div class="cocktails">
          <CocktailThumb v-for="cocktail in cocktails" :key="cocktail.idDrink" :cocktail="cocktail"/>
        </div>
      </div>
    </div>
  </AppLayout>
</template>

<style lang="sass" scoped>
@import "../assets/styles/main"

.select-wrapper
  padding-top: 50px

.select
  width: 220px

.text
  max-width: 516px
  margin: 0 auto
  padding-top: 50px
  color: $textMuted
  line-height: 36px
  letter-spacing: 0.1em

.img
  margin-top: 60px

.cocktails
  display: flex
  justify-content: center
  align-items: center
  flex-wrap: wrap
  margin-top: 60px
  max-height: 400px
  overflow-y: auto

.cocktails::-webkit-scrollbar
  width: 2px
  height: 12px

</style>