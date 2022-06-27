<template>

  <h1 v-if="!pokemon">Loading...</h1>

  <div v-else>
    <h1>¿Quien es este pokemon?</h1>
    
    <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon" />
    <PokemonOptions  :pokemons="pokemonArr" @selection="checkAnswer($event)" />

    <div v-if="showAnswer" class="fade-in">
      <h2>{{ message }}</h2>
      <button @click="newGame" >Nuevo juego</button>
    </div>
  </div>


</template>

<script>
import PokemonPicture from '@/components/PokemonPicture.vue'
import PokemonOptions from '@/components/PokemonOptions.vue'
import getPokemonOptions from '@/helpers/getPokemonOptions'



export default {
  components: { PokemonPicture, PokemonOptions },

  data() {
    return {
      pokemonArr: [],
      pokemon: null,
      showPokemon: false,
      showAnswer: false,
      message: ''
    }
  },
  methods: {
    async mixPokemonsArray() {
      this.pokemonArr = await getPokemonOptions()

      const randomIndex = Math.floor( Math.random() * 4 )
      this.pokemon = this.pokemonArr[ randomIndex ]
    },

    checkAnswer( pokemonId ) {
      this.showPokemon = true
      this.showAnswer = true
      if(pokemonId === this.pokemon.id){
        this.message = `Correcto, es ${ this.pokemon.name }!!`
      }else {
        this.message = `Oops, era ${ this.pokemon.name }!!`
      }
    },

    newGame() {
      this.pokemonArr = []
      this.pokemon = null
      this.showPokemon = false
      this.showAnswer = false
      this.message = ''

      this.mixPokemonsArray()
    }
  },
  mounted() {
    this.mixPokemonsArray()
  }

}
</script>