<template>
	<h1 v-if="!pokemon">Cargando...</h1>
	<div v-else>
		<h1>¿Cuál es este pokémon?</h1>
  		<PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon" />
  		<PokemonOptions :pokemons="pokemonArr" @selection="checkAnsqwer" />

		<template v-if="showAnswer">
			<h2 class="fade-in"> {{ message }} </h2>
			<button @click="newGame">
				Nuevo Juego
			</button>
		</template>
		

	</div>
</template>

<script>
import PokemonOptions from "../components/PokemonOptions.vue";
import PokemonPicture from "../components/PokemonPicture.vue";

import getPokemonOptions from "@/helpers/getPokemonOptions";

//console.log(getPokemonOptions())
export default {
	components: {
    	PokemonPicture,
    	PokemonOptions,
  	},
  	data() {
    	return {
			pokemonArr: [], //Es reactivo. Vue va a estar pendiente de cualquier cambio que suceda en él y cuando cambie volverá a
			//renderizar lo que sea necesario.
			pokemon: null,
			showPokemon: false,
			showAnswer: false,
			message: ''
    	};
  	},
  	methods: {
    	async mixPokemonArray() {
			this.pokemonArr = await getPokemonOptions()
			const numeroAleatorio = Math.floor( Math.random() * 4 )
			this.pokemon = this.pokemonArr[ numeroAleatorio ]
		},
		checkAnsqwer( selectedId ){
			this.showPokemon = true
			this.showAnswer = true

			if( selectedId === this.pokemon.id ) {
				this.message = `Correcto, es ${ this.pokemon.name }`
			} else {
				this.message = `Oops. era ${ this.pokemon.name }`
			}
		},
		newGame(){
			this.showPokemon = false
			this.showAnswer = false
			this.pokemonArr = []
			this.pokemon = null
			this.mixPokemonArray()
		}
  	},
	mounted(){
		this.mixPokemonArray()
	}
};
</script>
