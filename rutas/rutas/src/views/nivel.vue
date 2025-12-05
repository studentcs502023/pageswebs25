<!-- <template>
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
</template> -->


<template>
  <div class="contenedor">

    <div class="encabezado">
      <h1>Selecciona el Nivel</h1>
      <p class="categoria">Categoría: {{ respuesta }}</p>
    </div>

    <div class="niveles">

      <!-- TARJETA FÁCIL -->
      <div class="card" @click="seleccionarNivel('facil')">
        <img src="https://i.gifer.com/7Im6.gif" alt="Fácil" class="gif" />
        <h2>Fácil</h2>
        <p>Palabras sencillas, tienes 14 intentos, si la palabra es correcta se oculta</p>
      </div>

      <!-- TARJETA MEDIO -->
      <div class="card" @click="seleccionarNivel('medio')">
        <img src="https://i.gifer.com/4Q2O.gif" alt="Medio" class="gif" />
        <h2>Medio</h2>
        <p>Un poco más difícil, tienes 10 intentos con una pista</p>
      </div>

      <!-- TARJETA DIFÍCIL -->
      <div class="card" @click="seleccionarNivel('experto')">
        <img src="https://i.gifer.com/3aW3.gif" alt="Difícil" class="gif" />
        <h2>Difícil</h2>
        <p>Palabras largas y complejas, tienes 9 intentos solo tienes una pista en el octavo intento</p>
      </div>

    </div>
<div class="buton">
     <q-btn class="glossy" rounnded color="deep-orange" 
     label="jugar ahora" to="/play"/>
</div>


  </div>
</template>
<style >
/* Fondo general blanco */
.contenedor {
  background: #ffffff;
  min-height: 100vh;
  padding: 40px 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* Encabezado */
.encabezado {
  text-align: center;
  margin-bottom: 40px;
}

.encabezado h1 {
  font-size: 3rem;
  margin: 0;
  color: #222;
}

.categoria {
  font-size: 1.4rem;
  color: #555;
}

/* Tarjetas de niveles */
.niveles {
  display: flex;
  gap: 25px;
  justify-content: center;
  flex-wrap: wrap;
}

/* Cada tarjeta */
.card {
  background: white;
  width: 260px;
  padding: 20px;
  border-radius: 20px;
  text-align: center;
  cursor: pointer;
  border: 2px solid #eee;
  transition: 0.25s ease;
  box-shadow: 0 4px 12px rgba(0,0,0,0.06);
}

.card:hover {
  transform: translateY(-6px);
  box-shadow: 0 10px 20px rgba(0,0,0,0.15);
  border-color: #d2d2d2;
}

/* GIFs */
.gif {
  width: 120px;
  height: 120px;
  object-fit: contain;
  margin-bottom: 10px;
}

/* Títulos de tarjetas */
.card h2 {
  font-size: 1.8rem;
  color: #222;
  margin-bottom: 5px;
}

/* Subtexto */
.card p {
  color: #666;
  font-size: 1rem;
}
</style>



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

// function seleccionarNivel(nivel) {
//   localStorage.setItem("nivelSeleccionado", nivel);

//   if (nivel === "facil") nivelS.value = nivelF;
//   else if (nivel === "medio") nivelS.value = nivelM;
//   else if (nivel === "experto") nivelS.value = nivelE;

//   console.log("Nivel seleccionado:", nivelS.value);

//   // ❌ NO LLAMAR guardarPalabrasPorCategoria() aquí
// }

// function seleccionarNivel(nivel) {
//   localStorage.setItem("nivelSeleccionado", nivel);

//   if (nivel === "facil") nivelS.value = nivelF;
//   else if (nivel === "medio") nivelS.value = nivelM;
//   else if (nivel === "experto") nivelS.value = nivelE;

//   console.log("Nivel seleccionado:", nivelS.value);

//   // ✔ Ahora sí: guardar palabras correctas según el nivel y categoría
//   guardarPalabrasPorCategoria();
// }}

function seleccionarNivel(nivel) {
  // Guardar nivel
  localStorage.setItem("nivelSeleccionado", nivel);

  // Guardar intentos y pistas según nivel


  // Guardar lista de palabras según nivel
  if (nivel === "facil") nivelS.value = nivelF;
  else if (nivel === "medio") nivelS.value = nivelM;
  else if (nivel === "experto") nivelS.value = nivelE;

  // Guardar palabras específicas de la categoría
  guardarPalabrasPorCategoria();

  console.log("Reglas del nivel:", reglas);
  console.log("Nivel seleccionado:", nivel);
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

// function guardarPalabrasPorCategoria() {
//   const categoria = respuesta.value;

//   const encontrado = nivelS.value.find(obj => {
//     const key = Object.keys(obj)[0];
//     return key === categoria;
//   });

//   if (!encontrado) {
//     console.warn("Categoría no encontrada:", categoria);
//     return;
//   }

//   const palabras = encontrado[categoria];

//   localStorage.setItem("palabrasCategoria", JSON.stringify(palabras));
//   console.log("PALABRAS GUARDADAS:", palabras);
// }




// Nivel Fácil (ya lo tienes)
const nivelF = [
  { frutas: ["manzana", "pera", "sandía", "plátano", "uva"] },
  { animales: ["leon", "perro", "gato", "vaca", "burro"] },
  { paises: ["africa", "colombia", "francia", "paris", "suecia"] },
  { deportes: ["futbol", "basketball", "voleibol", "tenis", "natacion"] },
  { peliculas: ["titanic", "avatar", "inception", "matrix", "frozen"] },
  { ciencia: ["atomos", "gravedad", "energia", "fotosintesis", "celula"] }
  
];

// Nivel Medio (palabras un poco más largas o menos comunes)
const nivelM = [
  { frutas: ["kiwi", "mango", "maracuya", "cereza", "guayaba"] },
  { animales: ["jirafa", "hipopotamo", "canguro", "delfin", "tortuga"] },
  { paises: ["argentina", "uruguay", "australia", "dinamarca", "portugal"] },
  { deportes: ["esgrima", "badminton", "hockey", "triathlon", "escalada"] },
  { peliculas: ["interstellar", "gladiator", "casablanca", "amelie", "fargo"] },
  { ciencia: ["quimica", "biologia", "geologia", "astronomia", "botanica"] }
];

// Nivel Experto (palabras largas, complejas o técnicas)
const nivelE = [
  { frutas: ["lichi", "pitahaya", "physalis", "kumquat", "rambutan"] },
  { animales: ["ornitorrinco", "axolote", "pangolin", "narval", "cacatua"] },
  { paises: ["kazajistan", "liechtenstein", "macedonia", "azerbaiyán", "bhután"] },
  { deportes: ["pentatlon", "heptatlon", "curling", "escalada", "motocross"] },
  { peliculas: ["cienciadelasficciones", "elorigen", "hereditary", "memento", "psychonauts"] },
  { ciencia: ["microbiologia", "neurociencia", "electromagnetismo", "biotecnologia", "astrofisica"] }
];


</script>