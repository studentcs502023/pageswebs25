<template>
    <div>

    <div class="title">
        <h1></h1>
        <p>{{ respuesta }}</p>
    </div>

    <div class="niveles"  >
        <div class="facil">
<button @click="seleccionarNivel('facil')">Fácil</button>
<button @click="seleccionarNivel('medio')">Medio</button>
<button @click="seleccionarNivel('experto')">Difícil</button>


        </div>
        <div class="medio"  >

        </div>
        <div class="dificil">

        </div>
    </div>

    </div>
</template>

<style></style>

<script setup>
import { onMounted, ref } from 'vue';

// const respuesta= ref("")
// onMounted(() => {
//   const  data = localStorage.getItem('categoriaSeleccionada')
//  console.log("Categoría recibida:", data)
// respuesta.value = data
// })
// function guardarPalabrasPorCategoria(respuesta, nivelF) {
//   const categoria = respuesta.value;

//   // Buscar el objeto que tiene esa categoría como key
//   const encontrado = nivelF.find(obj => {
//     const key = Object.keys(obj)[0];
//     return key === categoria;
//   });

//   if (!encontrado) {
//     console.warn("Categoría no encontrada:", categoria);
//     return;
//   }

//   const palabras = encontrado[categoria];

//   // Guardar en LocalStorage
//   localStorage.setItem('palabrasCategoria', JSON.stringify(palabras));
//   console.log("PALABRAS GUARDADAS:", palabras);

//   return palabras; // opcional
// }

const nivelS = ref([]);

function seleccionarNivel(nivel) {
  localStorage.setItem("nivelSeleccionado", nivel);

  if (nivel === "facil") nivelS.value = nivelF;
  else if (nivel === "medio") nivelS.value = nivelM;
  else if (nivel === "experto") nivelS.value = nivelE;

  console.log("Nivel seleccionado:", nivelS.value);

  // ❌ NO LLAMAR guardarPalabrasPorCategoria() aquí
}





const respuesta = ref("");
onMounted(() => {

  const nivel = localStorage.getItem("nivelSeleccionado");

  if (!nivel) {
    console.warn("No hay nivel en localStorage");
    return;
  }

  console.log("Nivel recibido:", nivel);

  const data = localStorage.getItem("categoriaSeleccionada");
  respuesta.value = data;
});


function guardarPalabrasPorCategoria() {
  const categoria = respuesta.value;

  const encontrado = nivelS.value.find(obj => {
    const key = Object.keys(obj)[0];
    return key === categoria;
  });

  if (!encontrado) {
    console.warn("Categoría no encontrada:", categoria);
    return;
  }

  const palabras = encontrado[categoria];

  localStorage.setItem("palabrasCategoria", JSON.stringify(palabras));
  console.log("PALABRAS GUARDADAS:", palabras);
}



// Nivel Fácil (ya lo tienes)
const nivelF = [
  { frutas: ["manzana", "pera", "sandía", "plátano", "uva"] },
  { animales: ["leon", "perro", "gato", "vaca", "burro"] },
  { paises: ["africa", "colombia", "francia", "paris", "suecia"] },
  { deportes: ["futbol", "basketball", "voleibol", "tenis", "natacion"] },
  { peliculas: ["titanic", "avatar", "inception", "matrix", "frozen"] },
  { ciencias: ["atomos", "gravedad", "energia", "fotosintesis", "celula"] }
];

// Nivel Medio (palabras un poco más largas o menos comunes)
const nivelM = [
  { frutas: ["kiwi", "mango", "maracuya", "cereza", "guayaba"] },
  { animales: ["jirafa", "hipopotamo", "canguro", "delfin", "tortuga"] },
  { paises: ["argentina", "uruguay", "australia", "dinamarca", "portugal"] },
  { deportes: ["esgrima", "badminton", "hockey", "triathlon", "escalada"] },
  { peliculas: ["interstellar", "gladiator", "casablanca", "amelie", "fargo"] },
  { ciencias: ["quimica", "biologia", "geologia", "astronomia", "botanica"] }
];

// Nivel Experto (palabras largas, complejas o técnicas)
const nivelE = [
  { frutas: ["lichi", "pitahaya", "physalis", "kumquat", "rambutan"] },
  { animales: ["ornitorrinco", "axolote", "pangolin", "narval", "cacatua"] },
  { paises: ["kazajistan", "liechtenstein", "macedonia", "azerbaiyán", "bhután"] },
  { deportes: ["pentatlon", "heptatlon", "curling", "escalada", "motocross"] },
  { peliculas: ["cienciadelasficciones", "elorigen", "hereditary", "memento", "psychonauts"] },
  { ciencias: ["microbiologia", "neurociencia", "electromagnetismo", "biotecnologia", "astrofisica"] }
];


</script>