<template>
  <div id="app">
    <div class="logo">
      <img src="./assets/pokemon_logo.png" alt="">
    </div>
    <div class="div-search">
      <input class="input is-rounded input-search" type="text" placeholder="Buscar Pokémon" v-model="busca">
    </div>
    <div class="pokemons">
      <div v-for="(pokemon, index) in resultadoBusca" :key="pokemon.url">
        <PokemonCard :name="pokemon.name" :url="pokemon.url" :num="index+1"/>
      </div>
    </div>
    
  </div>
</template>

<script>
import axios from 'axios';
import PokemonCard from './components/PokemonCard.vue';


export default {
  name: 'App',
  data(){
    return{
      pokemons: [],
      busca: ''
    }
  },
  components: {
    PokemonCard
  },
  created: function() {
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(res => {
      
      this.pokemons = res.data.results;
      console.log(this.pokemons);
    })
  },
  methods: {
    filterItems: function(query) {
        return this.pokemons.filter(function(pokemon) {
        return pokemon.name.toLowerCase().indexOf(query.toLowerCase()) > -1;
    })
      
  }
  },
  computed: {
    resultadoBusca: function(){
      if(this.busca == '' || this.busca == ' '){
        return this.pokemons;
      }
      else{
        return this.filterItems(this.busca);
      }
      
    }
  }
}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  padding: 60px 0;
  background-color: #ecf0f1;
  min-height: 100vh;
}
.pokemons{
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
.logo{
  height: 200px;
  position: relative;
  margin: 25px 0;
}
.logo img{
  height: 100%;
}
.input-search{
  width: 300px;
  
}
.input-search:focus{
  border: none;
}
.div-search{
  padding: 25px 0;
}
</style>
