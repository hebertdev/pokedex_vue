<template  >

	<div>
		<div>

			<div class="container-pokebolla" v-if="loaded==false">
				<div class="pokeball">
					<div class="detalle">
					</div>
				</div>
			</div>

			<div id="container-all-pokemon-detail"  v-if="loaded==true">
				<div class="container-detail-pokemon" :style="estiloGradient" >
					<div style="height: 50px;">
					</div>
					<div class="max-container">
						<div class="header-name-pokemon">
							<nuxt-link  :to="`/pokemon/${previousPoke.id}`">
								<span class="icon-next-previous"  v-on:click="loaded=false"> < </span>	
							</nuxt-link>
							
							<h1 class="title-name-pokemon">{{pokemon.name}}</h1>
							
							<nuxt-link  :to="`/pokemon/${nextPoke.id}`" >
								<span class="icon-next-previous" v-on:click="loaded=false"> > </span>	
							</nuxt-link>
						</div>
						<div class="container-info-pokemon">
							<img v-bind:src="rutaImg" alt="" class="img-pokemon">


							<div style="display: flex;justify-content: center;align-items: center;">
								<div class="container-type-pokemon" v-for="type in types"   style="display: flex;width: 140px;" >
									<p v-for="typex in typesall" v-if="typex.name===type.type.name"  :style="typex.estilos" >
										<b>{{type.type.name}}</b>

									</p>
								</div>
							</div>

						</div>

					</div>
				</div>
			</div>
		</div>

	</div>

</template>
<script>

	import {route} from 'vue-router'


	export default {
		head() {
			return {
				title: "detail"
			};
		},

		components:{

		},



		data() {
			return {
				nextPoke:'',
				previousPoke:'',
				degradado: '',
				unarutadefinia : this.$route.path,
				pokemon:'',
				specie:'',
				loaded:false,
				rutaImg:'',
				types:'',


				estiloGradient:{
					'background':'white',
					
				},



				typesall:[
				{
					'name':'fire',
					estilos:{
						'background':'#f08030',
					}

				},
				{
					'name':'water',
					estilos:{
						'background':'#82c7e8',
					}
				},
				{
					'name':'bug',
					estilos:{
						'background':'#afd9a4',
					}
				},
				{
					'name':'normal',
					estilos:{
						'background':'#c7b49f',
					}
				},
				{
					'name':'fighting',
					estilos:{
						'background':'#D56723',
					}
				},
				{
					'name':'flying',
					estilos:{
						'background':'#d3e0ff',
					}
				},
				{
					'name':'poison',
					estilos:{
						'background':'#B97FC9',

					}					},
					{
						'name':'ground',
						estilos:{
							'background':'#d5be7a',
						}
					},
					{
						'name':'rock',
						estilos:{
							'background':'#b9b189',
						}
					},
					{
						'name':'ghost',
						estilos:{
							'background':'#6f70a7',
						}
					},
					{
						'name':'steel',
						estilos:{
							'background':'#b3b2b4',
						}
					},
					{
						'name':'grass',
						estilos:{
							'background':'#9BCC50',
						}
					},
					{
						'name':'electric',
						estilos:{
							'background':'#EED535',
						}
					},
					{
						'name':'psychic',
						estilos:{
							'background':'#f785c8',
						}
					},
					{
						'name':'ice',
						estilos:{
							'background':'#bce1f3',
						}
					},
					{
						'name':'dragon',
						estilos:{
							'background':'#3c9fbb',
						}
					},
					{
						'name':'dark',
						estilos:{
							'background':'#767083',
						}
					},
					{
						'name':'fairy',
						estilos:{
							'background':'#f6bfc4',
						}
					},
					]
				};
			},

			methods: {
				async getUrl(){
					try {

						console.log(this.loaded)

						this.ruta3 = this.$route.path
						let paths = this.ruta3.split('/');
						let pokedetail = paths[paths.length-1];


						let pokemon = await this.$axios.$get(`/pokemon/${pokedetail}/`);



						let especies = await this.$axios.$get(`/pokemon-species/${pokedetail}/`)

						let next =  pokemon.id + 1
						let previous =  pokemon.id - 1

						let nextPokemon = await this.$axios.$get(`/pokemon/${next}/`)
						let previousPokemon = await this.$axios.$get(`/pokemon/${previous}/`)
						console.log(previousPokemon)
						

						this.nextPoke = nextPokemon
						this.previousPoke = previousPokemon
						


						



						let types = pokemon.types

						let idpokemon = await ('000' + pokemon.id).slice(-3);

						let rutaimgpokemon = `https://assets.pokemon.com/assets/cms2/img/pokedex/detail/${idpokemon}.png`

						let color1 = types



						var alltypesx = this.typesall

						var colors = []

						if (types.length==2){
							let typeone = types[0].type.name
							let typetwo = types[1].type.name
							

							for (const prop2 in alltypesx){

								for(const prop3 in types){

									let pokemonType = types[prop3].type.name
									let pokemonTypes = alltypesx[prop2].name
									let backgroundColors = alltypesx[prop2].estilos.background

									if(pokemonType==pokemonTypes){
										console.log(backgroundColors)
										colors.push(backgroundColors)
										

										var backgroundLayout = `linear-gradient(to top, ${colors[0]} , ${colors[1]} ,  ${colors[0]} )`
										console.log(backgroundLayout)

										this.degradado = backgroundLayout
										this.estiloGradient.background = backgroundLayout

									}

								}
							}

						}

						else{
							let typeone = types[0].type.name
							for (const prop2 in alltypesx){


								let pokemonTypes = alltypesx[prop2].name
								let backgroundColors = alltypesx[prop2].estilos.background

								if(typeone==pokemonTypes){
									console.log(backgroundColors)
									colors.push(backgroundColors)

									var backgroundLayout = `linear-gradient(to top, ${colors[1]} ,  ${colors[0]})`
									console.log(backgroundLayout)

									this.degradado = backgroundLayout
									this.estiloGradient.background = backgroundColors




								}

								
							}
						}


						

						this.types = types
						this.pokemon = pokemon
						this.specie = especies
						this.rutaImg = rutaimgpokemon

						this.loaded = true

						console.log(this.loaded)


          //console.log(especies.flavor_text_entries[27].language)
          //console.log(especies.flavor_text_entries[3].language)

      } catch (e) {
      	console.log(e)


      }
  }

},

mounted: function(){
	this.getUrl()
},

};
</script>

<style>


/* animacion pokebola */
.container-pokebolla{
	width: 100%;
	height: 100vh;
	position: fixed;
	background: white;
	display: flex;
	justify-content: center;
	align-items: center;
}

.pokeball {
	width: 9rem;
	height: 9rem;
	background-color: white;
	border: solid .5em black;
	border-radius: 50%;
	margin: auto;
	animation-name: captura;
	animation-duration: 1s;
	animation-iteration-count: infinite;
	animation-play-state: running;
	
}

.pokeball::before {
	content: "";
	width: 9rem;
	height: 4.5rem;
	background-color: red;
	position: absolute;
	border-radius: 4.5rem 4.5rem 0 0;
}
.detalle {
	width: 9rem;
	height: 1rem;
	background-color: black;
	position: relative;
	margin-top: 4rem;
}

.detalle::before {
	content: "";
	width: 4rem;
	height: 4rem;
	background-color: white;
	position: absolute;
	border: solid .5rem black;
	border-radius: 50%;	
	margin-top: -2rem;
	margin-left: 2rem;
}

.pokeball::after {
	content: "";
	width: 3rem;
	height: 3rem;
	background-color: #7F8C8D;
	position: absolute;
	border-radius: 50%;
	margin-top: -2rem;
	margin-left: 3rem;
	animation-name: parpadeo;
	animation-duration: 1s;
	animation-iteration-count: infinite;
	animation-play-state: running;
}


@keyframes captura {
	0%{
		transform: translate(0rem) rotate(0deg);
	}
	30%{
		transform: translate(1rem) rotate(50deg);
	}
	60%{
		transform: translate(-1rem) rotate(-50deg);
	}
	100%{
		transform: translate(0rem) rotate(0deg);
	}
}

@keyframes parpadeo {
	0%{
		background-color: #7F8C8D;
	}
	50% {
		background-color: red;
	}
	100%{
		background-color: #7F8C8D;
	}
	
}
/* fin*/

a{
	text-decoration: none;
}
.icon-next-previous{
	color: white;
	font-size: 30px;
	font-weight: 600;
	padding: 10px;
	cursor: pointer;
	display: block;
	width: 35px;
	height: 35px;
	display: flex;
	justify-content: center;
	align-items: center;
	border-radius: 100%;
}
.icon-next-previous:hover{
	background: rgba(0,0,0,0.1);
}

.container-detail-pokemon{
	max-width: 100%;
	width: 100%;
	margin: auto;
	height: 100vh;
	background: rgb(104, 144, 240);
	
	

	/**background: linear-gradient(to right,  #4e73df78 ,#8360c3cc);**/ 
}

.max-container{
	max-width: 1100px;
	width: 90%;
	margin: auto;
	
}

.header-name-pokemon{
	width: 100%;
	height: 40px;
	background: rgba(0,0,0,0.1);
	display: flex;
	justify-content: space-between;
	align-items: center;
}

.title-name-pokemon{
	font-size: 30px;
	color: #333333;
	color: white;
	text-align: center;

}

.img-pokemon{
	margin: auto;
	display: block;
}

.container-info-pokemon{
	width: 100%;
	background: white;
}

.container-type-pokemon{
	display: flex;
	align-items: center;
	justify-content: center;
	width: 100%;
}
.container-type-pokemon p{
	width: 140px;
	
	text-align: center;
	margin: 5px;
	color: #00000069;
	font-weight: 600;
	padding: 2px;
	letter-spacing: 1px;
}

/* ani*/
</style>