<template>
  <div class="about">
    <select ref="select" @change="select($refs.select.value)">
    <option>-</option>
    <option v-for="poke in pokes" :key="poke.name" :value="poke.url">{{poke.name}}</option>
    </select>
    <Pokemon v-bind:url="url" />
  </div>
</template>


<script>
import Pokemon from '@/components/Pokemon.vue';
export default {
  name: 'Pokedex',
  components: {
    Pokemon
  },
  data: function(){
    return {
      pokes: [],
      url: "",
      select: function(value){
        this.url = value
      }
    }
  },
  async mounted(){
    
    const response = await fetch('https://pokeapi.co/api/v2/pokemon?limit=151')
    const info = await response.json()
    this.pokes = info.results
      }
}
</script>