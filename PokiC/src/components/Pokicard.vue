<script setup>
import { ref, onMounted } from 'vue'
import { getPokemon, getPokemonHeight, getPokemonWeight, getPokemonById, getPokemonWeaknesses } from "../api/pokemon.js";

// export default {
//   data() {
//     return {
//       imagePath: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRXcXbv9weIO1Scb5vGfHyhd8KhjE2dWqM6LyrJxphOmQ&s=10' // O una ruta local en /public
//     }
//   }
// }

// const titulopokemon = "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRXcXbv9weIO1Scb5vGfHyhd8KhjE2dWqM6LyrJxphOmQ&s=10"
let pokemon = ref({})
let altura = ref(0)
let peso = ref(0)
let pokemonId = ref([])
let weaknesses = ref([])

getPokemon('ditto').then((data) => {
    pokemon.value = data
})

onMounted(async () => { 
  pokemon.value = await getPokemon("mankey") 
  altura.value = await getPokemonHeight("mankey")
  peso.value = await getPokemonWeight("mankey")
  pokemonId.value = await getPokemonById("mankey")
//   weaknesses.value = await getPokemonWeaknesses("mankey") 
//debilidades de pokemon
const tipos = pokemon.value.types.map(t => t.type.name)
weaknesses.value = await getPokemonWeaknesses(tipos)
  console.log(pokemon.value) 
  console.log(altura.value)
  console.log(peso.value)
  console.log(pokemonId.value)
  console.log(weaknesses.value)
})

const debilidades = ref(['fuego', 'hielo', 'volador', 'psiquico'])

// Diccionario de colores por tipo
const coloresPorTipo = {
  fuego: '#F08030',
  agua: '#6890F0',
  planta: '#78C850',
  electrico: '#F8D030',
  hielo: '#98D8D8',
  lucha: '#C03028',
  veneno: '#A040A0',
  tierra: '#E0C068',
  volador: '#A890F0',
  psiquico: '#F85888',
  bicho: '#A8B820',
  roca: '#B8A038',
  fantasma: '#705898',
  dragon: '#7038F8',
  siniestro: '#705848',
  acero: '#B8B8D0',
  hada: '#EE99AC',
  normal: '#A8A878'
}

// Función para obtener el color o devolver uno por defecto
const obtenerColor = (tipo) => {
  return coloresPorTipo[tipo.toLowerCase()] || '#777777'
}

//porcentaje de estadisticas de pokemon


</script>

<template>
    <main>
        <img src="https://i.pinimg.com/736x/34/c1/e5/34c1e5d371d64a581b1902ec5c4509f4.jpg" width="280" height="210" class="imagen-p" />

        <section class="contenedor">

            <div class="pokemon-card">
                <h2 class="nombre-pokemon">{{ pokemon?.name }}</h2>
                
                <div class="informacion-pokemon">
                    <p>Altura: {{ altura }} m</p>
                    <p>Peso: {{ peso }} kg</p>
                </div>
                
                <img :src="pokemon?.sprites.other.home.front_default" :alt="pokemon?.name" width="500" class="imagen-pokemon" />
                
            </div>
            
            <div class="card_t_d">
                <!-- <h1 class="titulo-pokemon">Pokémon</h1> -->
                <p class="Id_pokemon"># {{ pokemonId }}</p>
                <h2 class="Tipos_p">Tipos del Pokemon</h2>
                <p v-for="type in pokemon?.types" :key="type.slot" class="Tipos">
                    {{ type.type.name }}
                </p>
    
                <h2>Debilidades de Pokemon</h2>
    
                <div class="debilidades_p">
                    <!-- <p v-for="weakness in weaknesses" :key="weakness" class="debilidades">
                        {{ weakness }}
                    </p> -->
                    <span
                        v-for="(tipo, index) in debilidades"
                        :key="index"
                        class="debilidades"
                        :style="{ backgroundColor: obtenerColor(tipo) }"
                        >
                        {{ tipo }}
                    </span>
                </div>
    
            </div>
    
        <div class="estadisticas">
        <h2>Estadísticas</h2>
    
        <div v-for="stat in pokemon?.stats" :key="stat.stat.name" class="estadistica">
            <p class="estadisticas_p">{{ stat.stat.name }}: {{ stat.base_stat }} / 255</p>
    
            <div class="barra-relleno">
                <div
                    class="barra-carga"
                    :style="{ width: (stat.base_stat / 255) * 100 + '%' }"
                ></div>
            </div>
        </div>
    </div>
    </section>
    </main>
</template>

<style lang="css">

@import url('https://fonts.googleapis.com/css2?family=Boogaloo&family=Fruktur:ital@0;1&family=Italianno&family=Rock+Salt&display=swap');

html{
    background: linear-gradient(to left, transparent 0%, #e68989 70%);
    font-family: "Rock Salt", cursive;
    font-weight: 400;
    font-style: normal;
}

/* Informacion del Pokemon*/
.imagen-p{
    filter: brightness(1,9);
    mix-blend-mode: multiply; /* para eliminar el fondo de la imagen */
    display: block;
    margin: 0 auto;
}

.contenedor{
    display: grid;
    grid-template-columns: repeat(3, 1fr); 
}

.imagen-pokemon{
    width: 100%;
    object-fit: contain;
    display: block;
    margin-bottom: 16rem;
}

.pokemon-card {
    position: relative; /* Referencia para el texto absoluto */
    width: 410px;
    height: 550px;
    border-radius: 16px;
    overflow: hidden;
    /* background: linear-gradient(135deg, #fceabb 0%, #f8b500 100%); */
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
    background-color: #ff0000;
    margin: 0 auto; /* Centra la tarjeta horizontalmente */
}

.informacion-pokemon{
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    /* padding: 16px; */
    padding-top: 16rem; 
    color: #ffffff;
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    text-align: center;
}

.nombre-pokemon{
    text-align: center;
}
/*..........*/

/*Id, Tipos, debilidades del pokemon */
.Id_pokemon{
    font-size: 4rem;
    font-weight: bold;
    text-align: center;
    color:  #ff0000;
}

.Tipos_p{
    text-align: center;
}

.Tipos{
    text-align: center;
    background-color: #ff0000;
    display: inline-block;
    justify-content: center; /* Centra horizontalmente */
    align-items: center; 
    color: white;                 /*Color del texto*/
    padding: 12px 24px;           /*Espaciado interno (arriba/abajo, lados)*/
    font-family: sans-serif;      /*Tipo de fuente*/
    font-size: 16px;              /*Tamaño de la letra*/
    font-weight: bold;            /*Grosor del texto*/
    border-radius: 6px;           /*Esquinas redondeadas*/
    cursor: pointer;              /*Cambia el cursor a una mano al pasar por encima*/
    margin: 0 auto; 
    font-family: "Rock Salt", cursive;
}

.card_t_d{
    text-align: center;
}

.debilidades_p{
    display: flex;
    justify-content: center; /**para centrar las debilidades */
    flex-wrap: wrap;
    gap: 8px; /* Separa cada etiqueta */
}

.debilidades{
    padding: 6px 12px;
    border-radius: 16px;
    color: #ffffff;
    font-weight: bold;
    font-size: 1rem;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

/* .Tipos:hover {
  background-color: #155a8a;    
  transform: scale(1.10);       
} */
/**........ */

/* Estadísticas */
.estadisticas {
    text-align: center;
    display: flex;
    flex-direction: column;
    margin-right: 4rem;
}
.estadisticas_p {
    /* margin: 8px 0; */
    text-align: center;
    font-weight: bold;
    text-transform: capitalize;
    margin-bottom: 6px;
}

.barra-relleno {
    width: 100%;
    height: 18px;
    background: #fff;
    border: 1px solid #333;
    border-radius: 10px;
    overflow: hidden;          /* recorta el relleno con el borde redondeado */
}

.barra-carga {
    height: 100%;
    background: #ff0000;
    transition: width 0.6s ease;
}

/* Movil*/
@media (max-width: 480px) {
    .contenedor {
        grid-template-columns: repeat(1, 1fr);
    }

    .estadisticas {
        margin-right: 0;
    }
}
</style>