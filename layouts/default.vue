<template>
  <div>
    <div>
      <div class="header">
        <div class="maxheader">
          <nuxt-link to="/" style="display: flex;align-items: center;text-decoration: none;">
            <img
            class="logo-poke"
            src="@/static/img/pokebola.png"
            >
            <h2 class="logotxt">PokedEX AQP</h2>
          </nuxt-link>
          
          <div>
            <label for="inputsearch"><img
              class="icon-searchxd"
              src="@/static/img/search.png"
              v-on:click="opensearch=true"></label>
            </div>
          </div>
        </div>
        <div class="searchFORM" v-if="opensearch==true">
          <div class="max-search-form">
            <div style="display: flex;">
              <input type="text" id="inputsearch" class="search-input" placeholder="search for pokemon"  v-model="pokemon" autocomplete="off" onkeyup="javascript:this.value=this.value.toLowerCase();">
              <span style="font-size: 25px;
              width: 34px;
              height: 34px;
              display: flex;color: #666;cursor: pointer;" v-on:click="opensearch=false">x</span>
            </div>


          </div>

          <ul class="" v-if="pokemon.length>0" class="ul--result-search">
            <nuxt-link class="nuxt-link-poke" v-for="poke in SearchPokemon" :to="`/pokemon/${poke.name}`" v-bind:key="poke.id">
              <li  class="li-result-search" v-on:click="opensearch=false"> 
                {{poke.name}} 
              </li>
            </nuxt-link>

          </ul>
        </div>

        
      </div>
      <div>
        <nuxt />
      </div>
    </div>
  </template>
  <script>

    export default {
      components: {
      },

      data: function() {
        return {
         pokemonList:[],
         hola:'asdasd',
         pokemon:'',
         opensearch:false,
       };
     },

     methods: {
      async getPokemons(){
        try {


          let pokemons = await this.$axios.$get(`/pokemon?offset=0&limit=807`)

          this.pokemonList = pokemons.results

        } catch (e) {
          console.log(e)

        }
      }

    },

    computed: {
      SearchPokemon: function () {
        return this.pokemonList.filter((item) => item.name.includes(this.pokemon));
      }
    },


    created: function(){

      this.getPokemons()

    }





  }
</script>

<style>
*{
  margin:0;
  padding: 0;
}
html {
  font-family: 'Source Sans Pro', -apple-system, BlinkMacSystemFont, 'Segoe UI',
  Roboto, 'Helvetica Neue', Arial, sans-serif;
  font-size: 16px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
}



.header{
  width: 100%;
  height: 50px;
  background: transparent;
  position: fixed;
  border-bottom: 1px solid rgba(0,0,0,0.05);
  z-index: 2;

}
.maxheader{
  max-width: 1100px;
  width: 95%;
  margin:auto;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;

}

.logo-poke{
  width: 45px;
  margin:3px 0;
  display: block; 

}

.logotxt{
  font-weight: 600;
  color: #333333;
  font-size: 20px;
}

.icon-searchxd{
  width: 30px;
  height: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  background: rgba(0,0,0,0.1);
  padding: 5px;
  border-radius: 100%;
  transition: all 0.5s;
}

.icon-searchxd:hover{
  background: rgba(0,0,0,0.3);
}

.searchFORM{
  width: 100%;
  height: 100vh;
  background: white;
  z-index: 5;
  position: fixed;
}

.max-search-form{
  max-width: 1100px;
  width: 95%;
  margin: auto;
  height: 50px;
  background: white;
}

.search-input{
  display: block;
  width: 95%;
  padding: 10px;
  border: none;
  border-bottom: 2px solid rgba(0,0,0,0.09);
  margin: auto;
}

.search-input:focus{
  outline: none;
}

.ul--result-search{
  width: 100%;
  height: 80vh;
  background: white;
  overflow: auto;
  max-width: 1100px;
  margin:auto;
  list-style: none;
  padding: 0;
}

.li-result-search{
  padding: 5px;
  cursor: pointer;
  font-size: 20px;
  padding: 10px;
}

.li-result-search:hover{
  background: rgba(0,0,0,0.1);
}

.nuxt-link-poke{
  list-style: none;
  text-decoration: none;
  color: #333333;
}



</style>
