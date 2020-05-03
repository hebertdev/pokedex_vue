<template>
  <div>
    <div>
      <pokemonCard
      v-for="pokes in pokemon"
      :key="pokes.id"
      :pokes="pokes"
      />
    </div>
  </div>
</template>

<script>

  import pokemonCard from '~/components/pokemons/pokemonCard'

  export default {
    head(){
      return {
        title:'Lista de pokemon'
      }
    },
    components: {
      pokemonCard,
    },
    data: function() {
      return {
       pokemonList:[],
       
     };
   },

   asyncData ({ $axios }) {
    return $axios.get('/pokemon').then((res) => {

      let pokemon = (res.data.results);

      console.log(JSON.stringify(res.data.results))

      console.log(pokemon + 'holaas')

      return {
        'pokemon':pokemon,
      }
    })
  },


}
</script>

<style>

</style>
