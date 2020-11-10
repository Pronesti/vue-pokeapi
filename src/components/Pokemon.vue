<template>
  <div>
    <img class="pokeImg" v-bind:src="pokeImg" v-bind:alt="url" @click="changeSprite" />
    <div class="tableDiv">
      <table class="pokeTable">
        <thead>
          <tr>
            <th v-for="(item, index) in poke" :key="item.index">{{ index }}</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td class="pokeTd" v-for="(item, index) in poke" :key="index">
              {{ item }}
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "Pokemon",
  data: function() {
    return {
      poke: {},
      pokeImg:
        "https://cdn.iconscout.com/icon/free/png-256/pokemon-pokeball-game-go-34722.png",
    };
  },
  methods: {
    changeSprite: function (){
        if(this.pokeImg == this.pokeImgFront){
          this.pokeImg = this.pokeImgBack
        }else if(this.pokeImg == this.pokeImgBack){
          this.pokeImg = this.pokeImgFront
        }
      }
  },
  props: {
    url: String
  },
  watch: {
    url: async function(newUrl, oldUrl) {
      if (newUrl != "-" && newUrl != oldUrl) {
        const response = await fetch(this.url)
        const info = await response.json()
          this.pokeImgFront= info.sprites.front_default
          this.pokeImgBack= info.sprites.back_default
          this.pokeImg= this.pokeImgFront
        this.poke = {
          id: info.id,
          name: info.name,
          height: info.height,
          weight: info.weight,
          types: info.types[0].type.name,
          abilities: info.abilities.map(ability => (ability.ability.name)).join(',')
        }
        if (info.types.length > 1) {
          this.poke.types += "-" + info.types[1].type.name;
        }
        info.stats.forEach(stat=>{
          this.poke[stat.stat.name] = stat.base_stat  
        })

      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pokeImg {
  max-width: 6rem;
  max-height: 6rem;
}

table {
  margin-left: auto;
  margin-right: auto;
}

table td,
th {
  padding: 0.5rem;
}

.tableDiv td,
th,
tr {
  border: 1px solid black;
}

h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
