<script setup>
import axios from "axios"
import { ref, computed } from "vue"
import { useRoute } from "vue-router"
import { COCKTAIL_BY_ID } from '@/constants'
import ApplayOut from '../components/AppLayOut.vue'

const route = useRoute()

const cocktail = ref(null)
const cocktailId = computed(() => route.path.split('/').pop())

const ingredients = computed(() => {
  const ingredients = [];

  for (let i = 1; i <= 15; i++) {
    if (!cocktail.value[`strIngredient${i}`]) break

    const ingredient = {}
    ingredient.name = cocktail.value[`strIngredient${i}`]
    ingredient.measure = cocktail.value[`strMeasure${i}`]

    ingredients.push(ingredient)
  }
  return ingredients
})

async function getCocktail() {
  const data = await axios.get(`${COCKTAIL_BY_ID}${cocktailId.value}`)
  cocktail.value = data?.data?.drinks[0];
}

function goBack() {
  router.go(-1)
}
getCocktail()
</script>

<template>
  <div v-if='cocktail' class="wrapp">
    <ApplayOut :imgUrl='cocktail.strDrinkThumb'>
      <div class="wrapper">
        <div class="info">
          <div class="title">{{ cocktail.strDrink }}</div>
          <div class="line"></div>
          <div class="list">
            <div v-for='(item, key) in ingredients' :key='key' class="list-item">
              {{ item.name }}
              <template v-if="item.measure">
                |
                {{ item.measure }}
              </template>
            </div>
          </div>
          <div class="instructions">
            {{ cocktail.strInstructions }}
          </div>
        </div>
      </div>
    </ApplayOut>
  </div>
</template>

<style lang='sass' scoped>
@use '../assets/styles/main' as *
</style>