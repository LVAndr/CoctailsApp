<script setup>
import axios from "axios";
import {COCKTAIL_RANDOM, INGREDIENT_PIC} from "@/constans/api.js";
import { Swiper, SwiperSlide } from 'swiper/vue';
import 'swiper/css';
import AppLayout from "@/components/AppLayout.vue";
import {computed, ref} from "vue";



const cocktail = ref(null);
const ingredients = computed(()=> {
  const ingredients = [];
  for (let i=1; i <= 15; i++) {
    if (!cocktail.value[`strIngredient${i}`]) {
      break
    }

    const ingredient = cocktail.value[`strIngredient${i}`];

    ingredients.push(ingredient);
  }

  return ingredients;
})

async function getCocktail() {
  const data = await axios.get(`${COCKTAIL_RANDOM}`);
  cocktail.value = data?.data?.drinks[0];
}

getCocktail();
</script>

<template>
  <div class="wrap" v-if="cocktail">
    <AppLayout
        :imgUrl="`${cocktail.strDrinkThumb}`"
    >
      <div class="wrapper">
        <div class="info">
          <div class="title">{{cocktail.strDrink}}</div>
          <div class="line"></div>
          <div class="slider">
            <swiper
                :slides-per-view="3"
                :space-between="50"
                class="swiper"
            >
              <swiper-slide
              v-for="(ingredient, key) in ingredients"
              :key="key"
              class="cocktail-slide"
              >
                <img :src="`${INGREDIENT_PIC}${ingredient}-Small.png`" :alt="`${ingredient} image`">
                <div class="name">{{ingredient}}</div>
              </swiper-slide>

            </swiper>
          </div>
          <div class="instructions">
            {{cocktail.strInstructions}}
          </div>
        </div>
      </div>
    </AppLayout>
  </div>

</template>

<style scoped lang="sass">
@import "../assets/styles/main"

.slider
  padding: 50px 0

.swiper
  max-width: 586px

.cocktail-slide
  cursor: pointer


.name
  padding-top: 20px
</style>