<template>
  <div v-if="!pokemon" class="loader"></div>
  <div v-else class="else">
  <div class="vidas">Vidas: {{vidas}}</div>
  <div class="puntos">Puntaje: {{puntos}}</div>
    <h1>Quien es este pokemon?</h1>

    <PokemonPicture :pokemon-id="pokemon.id" :show-pokemon="showPokemon" />  
    <PokemonOptions :pokemons="pokemonsArr" @selection="checkAnswer"/>
    <template v-if="showAnswer" class="fade-in">
      <h2 class="message">{{message}}</h2>
      <button @click="newGame" >{{nameButton}}</button>
    </template>  
  </div>

</template>

<script>
import PokemonOptions from '@/components/PokemonOptions'
import PokemonPicture from '@/components/PokemonPicture'

import getPokemonOptions from '@/helpers/getPokemonOptions'



export default {

    components: {
        PokemonPicture,
        PokemonOptions
    },
    data(){
      return{
        pokemonsArr: [],
        pokemon: null,
        showPokemon: false,
        showAnswer: false,
        message: '',
        nameButton: '',
        vidas: '❤️❤️❤️',
        puntos: 0,
      }
    },

    methods: {
      async mixPokemonArr(){
        this.pokemonsArr = await getPokemonOptions()

        const rndInt = Math.floor(Math.random() * 4)
        this.pokemon = this.pokemonsArr[rndInt]
      },
      
      checkAnswer(pokemonId){
        this.showPokemon = true
        this.showAnswer = true
        this.nameButton = 'Nuevo Pokemon'
        if(pokemonId === this.pokemon.id){
          this.message = `Correcto! es ${this.pokemon.name}`
          this.puntos++
        } else{
          this.showPokemon = false

          this.vidas = this.vidas.slice(0, -1)
          this.message = `Oops! era ${this.pokemon.name}!!!` 
          setTimeout(() => {
            this.showPokemon = true
          }, 1000)
          if(this.vidas === ''){
            this.vidas = '💔'
            this.message = `PERDISTE!`
            this.nameButton = 'Nuevo Juego en 3 segundos'
            //reedirigir a la pagina de inicio si da click en el boton
            setTimeout(() => {
              window.location.href = '/'
            }, 3000)
            
          }
        } 
      },
      newGame(){
        this.showAnswer = false
        this.showPokemon = false
        this.pokemonsArr = []
        this.pokemon = null
        this.mixPokemonArr()
      }
    },
 
    mounted(){
      this.mixPokemonArr()
    }

}
</script>

<style scoped>
.else{
  height: 520px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.loader {
  color: #ffffff;
  font-size: 45px;
  text-indent: -9999em;
  overflow: hidden;
  width: 1em;
  height: 1em;
  border-radius: 50%;
  position: absolute;
  right: 50%;
  transform: translateZ(0);
  animation: mltShdSpin 1.7s infinite ease, round 1.7s infinite ease;
}

.vidas{
  font-size: 20px;
  /* poner en esquina superior izquierda*/
  position: absolute;
  top: 0;
  left: 0;
  margin-top: 15px;
  margin-left: 15px;
}

.puntos{
  font-size: 20px;
  /* poner en esquina superior derecha*/
  position: absolute;
  top: 0;
  right: 0;
  margin-top: 15px;
  margin-right: 15px;
}

.message{
  color: #ffffff;
  font-size: 18px;
  text-align: center;
  margin: 0;
  padding: 0;
  padding-bottom: 20px;
  transition: 1s;
}

button {
  width: 250px;
  background-color: #ffffff;
  border: 1px solid #ffffff;
  border-radius: 5px;
  padding: 5px;
  cursor: pointer;
  margin-left: 35px;
  /* margin-top: -40px; */
  transition: .5s;
}

button:hover{
    background-color: rgb(6, 152, 65);
    border-radius: 5px;
    border: 1px solid rgba(0, 0, 0, 0.2);
    color: #ffffff;
    width: 260px;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.2);
    font-size: 16px;
}


@keyframes mltShdSpin {
  0% {
    box-shadow: 0 -0.83em 0 -0.4em,
    0 -0.83em 0 -0.42em, 0 -0.83em 0 -0.44em,
    0 -0.83em 0 -0.46em, 0 -0.83em 0 -0.477em;
  }
  5%,
  95% {
    box-shadow: 0 -0.83em 0 -0.4em, 
    0 -0.83em 0 -0.42em, 0 -0.83em 0 -0.44em, 
    0 -0.83em 0 -0.46em, 0 -0.83em 0 -0.477em;
  }
  10%,
  59% {
    box-shadow: 0 -0.83em 0 -0.4em, 
    -0.087em -0.825em 0 -0.42em, -0.173em -0.812em 0 -0.44em, 
    -0.256em -0.789em 0 -0.46em, -0.297em -0.775em 0 -0.477em;
  }
  20% {
    box-shadow: 0 -0.83em 0 -0.4em, -0.338em -0.758em 0 -0.42em,
     -0.555em -0.617em 0 -0.44em, -0.671em -0.488em 0 -0.46em, 
     -0.749em -0.34em 0 -0.477em;
  }
  38% {
    box-shadow: 0 -0.83em 0 -0.4em, -0.377em -0.74em 0 -0.42em,
     -0.645em -0.522em 0 -0.44em, -0.775em -0.297em 0 -0.46em, 
     -0.82em -0.09em 0 -0.477em;
  }
  100% {
    box-shadow: 0 -0.83em 0 -0.4em, 0 -0.83em 0 -0.42em, 
    0 -0.83em 0 -0.44em, 0 -0.83em 0 -0.46em, 0 -0.83em 0 -0.477em;
  }
}

@keyframes round {
  0% { transform: rotate(0deg) }
  100% { transform: rotate(360deg) }
}
 
</style>