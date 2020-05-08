<template  >

	<div>
		<div>

			<div class="container-pokebolla" v-if="loaded==false">
				<div class="pokeball">
					<div class="detalle">
					</div>
				</div>

				<audio :src="require('@/static/img/pokedex.wav')" autoplay="" loop=""></audio>
			</div>

			<div id="container-all-pokemon-detail"  v-if="loaded==true" >
				<div class="container-detail-pokemon" :style="estiloGradient" >	

					<div class="container-pokedex-info">
						<div style="height: 55px;"></div>
						<div class="container-img-pokemon" style="height: 215px;position: relative;z-index: 1;">
							<img v-bind:src="rutaImg" alt="" class="img-pokemon" style="display: block;margin:auto;">
						</div>
						<div class="info-pokemon">
							<div style="height: 50px;"></div>
							<div style="display: flex;justify-content: center;align-items: center;">
								<div class="container-type-pokemon" v-for="type in types"  style="display: flex;width: 140px;"  >
									<p class="txt-type" v-for="typex in typesall" v-if="typex.name===type.type.name"  :style="typex.estilos" style="margin-bottom: 10px;" >
										<b>{{type.type.name}}</b>
										
									</p>
								</div>
							</div>
							<div class="pokedex-info-poke" style="padding: 15px;box-sizing: border-box;">
								<div class="container-txt-description-pokemon">
									<p style="color: #333333;"><b>{{descriptionPokemon}}</b></p>
								</div>

								<div style="width: 100%">

									<p :style="first_color" style="padding: 3px 5px;margin-top:10px;font-size: 19px;border-radius: 5px;"><b>PROFILE   </b></p>
									<div class="info-profile-pokemon">
										<div style="width: 49%;margin: 5px 0;">
											<p><b>Height:</b> <span> {{height}}m </span> </p>
										</div>
										<div style="width: 49%;margin: 5px 0;">
											<p><b>Weight:</b> <span> {{weight}}kg </span> </p>
										</div>
										<div style="width: 49%;margin: 5px 0;">
											<p><b>Catch Rate:</b> <span> {{capture}}% </span> </p>
										</div>
										
										<div style="width: 49%;margin: 5px 0;">
											<p><b>Egg Groups:</b> <span> <p style="display: flex;" v-for="egg in specie.egg_groups"> -{{egg.name}}   </p> </span> </p>
										</div>
										
										<div style="width: 49%;margin: 5px 0;display: flex;" >
											<p><b>Abilities:</b> <span style=""> <p style="display: flex;" v-for="ability in pokemon.abilities"> -{{ability.ability.name}}   </p> </span> </p>
										</div>

									</div>
									
								</div>
								<div style="height: 30px;">
									
								</div>
							</div>
							<div class="header-name-pokemon" :style="first_color">
								<nuxt-link  :to="`/pokemon/${previousPoke.id}`">
									<span class="icon-next-previous"  v-on:click="loaded=false"> < </span>	
								</nuxt-link>

								<h1 class="title-name-pokemon">{{pokemon.name}} #{{pokemon.id}} </h1>

								<nuxt-link  :to="`/pokemon/${nextPoke.id}`" >
									<span class="icon-next-previous" v-on:click="loaded=false"> > </span>	
								</nuxt-link>
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
				title: this.pokemon.name
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
				loaded:true,
				rutaImg:'',
				types:'',
				descriptionPokemon:'',
				height:'',
				weight:'',
				capture:'',
				resultSearchvoz:'',




				estiloGradient:{
					'background':'white',
				},

				first_color:{
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

						this.ruta3 = this.$route.path
						let paths = this.ruta3.split('/');
						let pokedetail = paths[paths.length-1];

						let ahorsi = this.resultSearchvoz

						let pokemon = await this.$axios.$get(`/pokemon/${pokedetail}/`);

						
						




						let especies = await this.$axios.$get(`/pokemon-species/${pokedetail}/`)

						let next =  pokemon.id + 1
						let previous =  pokemon.id - 1

						if(next == 808){
							next = 1
						}

						if(previous==0){
							previous=807
						}







						let nextPokemon = await this.$axios.$get(`/pokemon/${next}/`)
						let previousPokemon = await this.$axios.$get(`/pokemon/${previous}/`)



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
										this.first_color.background = colors[1]

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

									colors.push(backgroundColors)

									var backgroundLayout = `linear-gradient(to top, ${colors[1]} ,  ${colors[0]})`

									


									this.degradado = backgroundLayout
									this.estiloGradient.background = backgroundColors
									this.first_color.background = backgroundColors

								}


							}
						}



						var descriptionPokemon = especies.flavor_text_entries
						var txtdescription = []
						for (const prop4 in descriptionPokemon){

							var espanishText = descriptionPokemon[prop4].language.name
							if (espanishText == 'es'){

								txtdescription.push(descriptionPokemon[prop4].flavor_text)



								this.descriptionPokemon = txtdescription[1]

							}

						}


						this.height = pokemon.height * 0.10
						this.weight = pokemon.weight * 0.10
						this.capture = especies.capture_rate * 0.10

						console.log(pokemon)
						
						
						this.types = types
						this.pokemon = pokemon
						this.specie = especies
						this.rutaImg = rutaimgpokemon

						var speech = new SpeechSynthesisUtterance(pokemon.name);

						speech.lang = 'en-GB';
						speechSynthesis.speak(speech)


						this.loaded = true


					} catch (e) {
						console.log(e)


					}
				},

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
		color: #333333;
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
		position: fixed;



		/**background: linear-gradient(to right,  #4e73df78 ,#8360c3cc);**/ 
	}
	.container-pokedex-info{
		max-width: 600px;
		height: 100vh;
		background: ;
		margin: auto;
	}
	.info-pokemon{
		background: white;
		width: 100%;
		height: 300px;
		margin-top:-60px;
		border-top-left-radius: 35px;
		border-top-right-radius: 35px;
		height: calc(100% - 210px);
		overflow: auto;
		position: relative;
	}

	.pokedex-info-poke{
		width: 100%;
		background: white;
		height: calc(100% - 100px);
		overflow: auto;
	}
	.header-name-pokemon{
		position: fixed;
		bottom: 0;
		background: orange;
		max-width: 600px;
		width: 100%;
		display: flex;
		justify-content: space-between;
		align-items: center;
		height: 50px;
		border-top: 1px solid rgba(0,0,0,0.1);
		color: #333333;

	}

	.header-name-pokemon h1{
		font-size: 21px;
	}

	.txt-type{
		display: block;
		width: 100%;
		text-align: center;
	}
	.info-profile-pokemon div p{
		font-size: 18px;
	}

	
</style>