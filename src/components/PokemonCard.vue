<template>
  <div class="card">
    <div class="card-image">
      <figure>
        <img :src="currentImg" alt="Placeholder image">
      </figure>
    </div>
    <div class="card-content">
      <div class="media">
        <div class="media-content">
          <p class="title is-4">{{name | upper}}</p>
          <p class="subtitle is-6">{{ pokemon.type }}</p>
        </div>
      </div>
      <div class="content">
        <button @click="mudarSprite" class="button is-danger is-rounded">Girar</button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  created: function(){
    axios.get(this.url).then(res => {
      this.pokemon.type = res.data.types[0].type.name;
      this.pokemon.img3d = res.data.sprites.other.home.front_default;
      this.pokemon.frontImg = res.data.sprites.front_default;
      this.pokemon.backImg = res.data.sprites.back_default;
      this.currentImg = this.pokemon.frontImg;
    })
  },
  data(){
    return{
      isFront: true,
      currentImg: '',
      pokemon: {
        type: "",
        backImg: "",
        imgProfile: ""
      }
    }
  },
  props: {
    num: Number,
    name: String,
    url: String
  },
  filters: {
    upper: function(value){
      var name = value;

      return name[0].toUpperCase() + name.substr(1);
    }
  },
  methods: {
    mudarSprite: function(){
      if(this.isFront){
        this.isFront = false;
        this.currentImg = this.pokemon.backImg;
      }
      else{
        this.isFront = true;
        this.currentImg = this.pokemon.frontImg;
      }
    }
  }
}
</script>

<style>
  .card{
    width: 200px;
    margin: 8px;
  }
  .card-content{
    padding: 8px;
  }
  .media{
    align-items: center;
  }
  .subtitle{
    color: #8c7ae6;
    text-transform: capitalize;
    font-weight: bold;
  }
</style>