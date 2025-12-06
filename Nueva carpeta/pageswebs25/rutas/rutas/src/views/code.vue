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
        <p>Palabras sencillas</p>
      </div>

      <!-- TARJETA MEDIO -->
      <div class="card" @click="seleccionarNivel('medio')">
        <img src="https://i.gifer.com/4Q2O.gif" alt="Medio" class="gif" />
        <h2>Medio</h2>
        <p>Un poco más difícil</p>
      </div>

      <!-- TARJETA DIFÍCIL -->
      <div class="card" @click="seleccionarNivel('experto')">
        <img src="https://i.gifer.com/3aW3.gif" alt="Difícil" class="gif" />
        <h2>Difícil</h2>
        <p>Palabras largas y complejas</p>
      </div>

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



<!-- <template>
    <div class="color">

        <div class="center">
<div class="tittle" style="display: flex; align-items: center; gap: 20px;">
<div style="display: flex; align-items: center; gap: 20px;">
    <pre style="margin: 0; ">
  +---+
  |   |
  O   |
 /|\  |
 / \  |
      |
=========
    </pre>

    <h1 style="margin: 0; ">AHORCADO</h1>

  
</div>

  


</div>



<div class="img">
    <img src="/public/iamage.png" alt="imagen">
</div>
<q-btn
  label="Play Now"
  to="/categorias"
  glossy
  color="purple"
/>


</div>


    </div>
</template>

<style>
.color{
    background: aliceblue; /* Gris muy oscuro con tono azulado */
    width: 100%;
    height: 87vh;
    overflow-y: hidden;
}

.center{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
     

}
.tittle{
    display: flex;
    flex-direction: column;
}

.img{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}
.img img{
    width: 500px;
    height: auto;
}

</style> -->
<template>
  <div class="inicio">

    <div class="contenido">
      <!-- Título + ASCII del ahorcado -->
      <div class="titulo">
        <pre class="ascii">
  +---+
  |   |
  O   |
 /|\  |
 / \  |
      |
=========
        </pre>

        <h1>AHORCADO</h1>
      </div>

      <!-- Imagen (opcional) -->
      <div class="imagen">
        <img src="/public/iamage.png" alt="Imagen del juego">
      </div>

      <!-- Botón -->
      <q-btn
        label="Play Now"
        to="/categorias"
        glossy
        color="purple"
        class="boton"
      />
    </div>

  </div>
</template>

<style scoped>
/* Fondo con ambiente de misterio + textura */
.inicio {
  width: 100%;
  height: 100vh;
  background: linear-gradient(rgba(10,10,10,0.7), rgba(10,10,10,0.7)),
              url("https://i.imgur.com/9N4eQhY.jpeg");
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
  overflow: hidden;
}

/* Contenedor central */
.contenido {
  text-align: center;
  display: flex;
  flex-direction: column;
  gap: 20px;
  align-items: center;
}

/* ASCII del ahorcado */
.ascii {
  font-size: 16px;
  line-height: 18px;
  color: #ffdfdf;
  text-shadow: 0px 0px 5px #ff9a9a;
}

/* Título */
.titulo h1 {
  margin: 0;
  font-size: 3rem;
  letter-spacing: 3px;
  text-shadow: 0px 0px 10px rgba(255,255,255,0.6);
}

/* Imagen del personaje */
.imagen img {
  width: 320px;
  max-width: 100%;
  border-radius: 12px;
  box-shadow: 0 0 15px rgba(255,255,255,0.2);
}

/* Botón */
.boton {
  margin-top: 20px;
  font-size: 20px;
  padding: 10px 20px;
}
</style>

<script setup>

</script>


<!-- <template>
  <q-layout view="lHh Lpr lFf" class="bg-white">


    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
      class="bg-grey-2"
    >

          <q-btn color="primary" label="Inicio" to="/inicio"/><br>
       <q-btn color="primary" label="Categorias" to="/categorias"/><br>
       <q-btn color="primary" label="Nivel" to="/nivel"/><br>
        <q-btn color="primary" label="Animales" to="/animales"/><br>
       <q-btn color="primary" label="Score" to="/score"/><br>
      
    </q-drawer>

    <q-page-container>
      <router-view />
      <h1>holacocmo fsad</h1>

    </q-page-container>
  </q-layout>
</template>

<script>
import { ref } from 'vue'

export default {
  name: 'MyLayout',

  setup () {
    const leftDrawerOpen = ref(false)

    function toggleLeftDrawer () {
      leftDrawerOpen.value = !leftDrawerOpen.value
    }

    return {
      leftDrawerOpen,
      toggleLeftDrawer
    }
  }
}
</script> -->



<template>
  <div>

    <!-- PALABRA EN FORMATO "_ _ _ _" -->
    <p class="palabra">
      <span v-for="(l, i) in letrasEncontradas" :key="i">
        {{ l }}
      </span>
    </p>

    <!-- TECLADO -->
    <div class="teclado">
      <button v-for="(letra, i) in teclado" :key="i" @click="presionarLetra(letra)">
        {{ letra }}
      </button>
    </div>

  </div>
</template>


<script setup>
import { onMounted, ref } from 'vue';

const teclado = ref([
  "a","b","c","d","e","f","g","h","i","j","k","l","m",
  "n","o","p","q","r","s","t","u","v","w","x","y","z"
]);
const respuesta = ref("");
const palabraAleatoria = ref("");
const letrasEncontradas = ref([]);
const keyfind = ref("");

onMounted(() => {
  const nivel = localStorage.getItem("nivelSeleccionado");
  const categoria = localStorage.getItem("categoriaSeleccionada");
  const palabrasCategoria = localStorage.getItem("palabrasCategoria");

  console.log("Nivel recibido:", nivel);
  console.log("Categoria recibida:", categoria);
  console.log("Palabras recibidas:", palabrasCategoria);

  if (!palabrasCategoria) {
    console.warn("No hay palabras guardadas");
    return;
  }

  const palabras = JSON.parse(palabrasCategoria);

  // Elegir palabra aleatoria:
  palabraAleatoria.value = palabras[Math.floor(Math.random() * palabras.length)];
console.log("esta es la palabra", palabraAleatoria.value)
  letrasEncontradas.value = palabraAleatoria.value.split("").map(() => "_");
});
function presionarLetra(letra) {
  keyfind.value = letra;

  // Revisar si esa letra está en la palabra
  for (let i = 0; i < palabraAleatoria.value.length; i++) {
    if (palabraAleatoria.value[i] === letra) {
      letrasEncontradas.value[i] = letra; // reemplaza el guion
    }
  }
}
</script>



























<template>
  <div class="juego-container">

    <!-- DIBUJO AHORCADO -->
    <div class="ahorcado">
      <pre class="dibujo" v-pre>{{ dibujoAhorcado }}</pre>
    </div>

    <!-- INTENTOS RESTANTES -->
    <div class="info-juego">
      <div class="intentos">
        <span class="emoji">❤️</span>
        <span class="contador">{{ intentosRestantes }}</span>
        <span class="texto">Intentos</span>
      </div>
    </div>

    <!-- PALABRA -->
    <div class="palabra-container">
      <p class="palabra">
        <span v-for="(letra, i) in letrasEncontradas" :key="i" class="letra-casilla">
          {{ letra }}
        </span>
      </p>
      <p class="mensaje">{{ mensaje }}</p>
    </div>

    <!-- TECLADO -->
    <div class="teclado">
      <button 
        v-for="(letra, i) in teclado" 
        :key="i" 
        @click="presionarLetra(letra)"
        :disabled="letrasUsadas.includes(letra) || juegoTerminado"
        :class="{
          'correcta': letrasCorrectas.includes(letra),
          'incorrecta': letrasIncorrectas.includes(letra),
          'disabled': letrasUsadas.includes(letra)
        }"
      >
        {{ letra }}
      </button>
    </div>

    <!-- BOTONES DE ACCIÓN -->
    <div class="botones-accion">
      <button @click="reiniciarJuego" class="btn-reiniciar">🔄 Reiniciar</button>
      <button @click="$router.push('/')" class="btn-inicio">🏠 Volver a Inicio</button>
    </div>

    <!-- REGLAS -->
    <div class="reglas-nivel">
      <h3>📋 Reglas del Nivel:</h3>
      <p>{{ reglasTexto }}</p>
    </div>

  </div>
</template>

<script setup>
import { onMounted, ref, computed } from 'vue';

// ==========================
// 🎨 DIBUJOS DEL AHORCADO
// ==========================
const dibujos = [
`
`,
`
|
|
|
|
|
`,
`
+---+
|
|
|
|
|
`,
`
+---+
|   O
|
|
|
|
`,
`
+---+
|   O
|   |
|
|
|
`,
`
+---+
|   O
|  /|
|
|
|
`,
`
+---+
|   O
|  /|\\
|
|
|
`,
`
+---+
|   O
|  /|\\
|  /
|
|
`,
`
+---+
|   O
|  /|\\
|  / \\
|
|
`
];

const dibujoAhorcado = ref(dibujos[0]);

// ==========================
// 🔤 LETRAS Y JUEGO
// ==========================
const teclado = ref([
  "a","b","c","d","e","f","g","h","i","j","k","l","m",
  "n","o","p","q","r","s","t","u","v","w","x","y","z"
]);

const palabraAleatoria = ref("");
const letrasEncontradas = ref([]);
const letrasUsadas = ref([]);
const letrasCorrectas = ref([]);
const letrasIncorrectas = ref([]);

const intentosRestantes = ref(0);
const juegoTerminado = ref(false);
const mensaje = ref("");

// ==========================
// 🎮 INICIAR JUEGO
// ==========================
function iniciarJuego() {
  const nivel = localStorage.getItem("nivelSeleccionado");
  const palabrasCategoria = localStorage.getItem("palabrasCategoria");

  // Reset total del juego
  letrasUsadas.value = [];
  letrasCorrectas.value = [];
  letrasIncorrectas.value = [];
  dibujoAhorcado.value = dibujos[0];
  juegoTerminado.value = false;
  mensaje.value = "";

  if (!palabrasCategoria) return;

  let palabras = [];
  try {
    palabras = JSON.parse(palabrasCategoria);
  } catch (e) {
    console.error("Error al convertir JSON:", e);
    return;
  }

  if (!Array.isArray(palabras) || palabras.length === 0) return;

  // Palabra aleatoria
  palabraAleatoria.value = palabras[Math.floor(Math.random() * palabras.length)];
  letrasEncontradas.value = palabraAleatoria.value.split("").map(() => "_");

  // Intentos según nivel
  if (nivel === "facil") intentosRestantes.value = 14;
  else if (nivel === "medio") intentosRestantes.value = 10;
  else if (nivel === "experto") intentosRestantes.value = 9;
}

onMounted(() => {
  iniciarJuego();
});

// ==========================
// ⌨ PRESIONAR LETRA
// ==========================
function presionarLetra(letra) {
  if (juegoTerminado.value || letrasUsadas.value.includes(letra)) return;

  letrasUsadas.value.push(letra);

  // Letra incorrecta
  if (!palabraAleatoria.value.toLowerCase().includes(letra)) {
    letrasIncorrectas.value.push(letra);
    intentosRestantes.value--;

    // Actualizar dibujo
    const errores = letrasIncorrectas.value.length;
    dibujoAhorcado.value = dibujos[Math.min(errores, dibujos.length - 1)];

    mensaje.value = `❌ Letra "${letra}" incorrecta. Te quedan ${intentosRestantes.value} intentos`;

    if (intentosRestantes.value <= 0) {
      juegoTerminado.value = true;
      mensaje.value = `💀 ¡Perdiste! La palabra era: "${palabraAleatoria.value}"`;
    }

    return;
  }

  // Letra correcta
  letrasCorrectas.value.push(letra);
  mensaje.value = `✅ ¡Correcto! Letra "${letra}" encontrada`;

  for (let i = 0; i < palabraAleatoria.value.length; i++) {
    if (palabraAleatoria.value[i].toLowerCase() === letra) {
      letrasEncontradas.value[i] = letra;
    }
  }

  if (!letrasEncontradas.value.includes("_")) {
    juegoTerminado.value = true;
    mensaje.value = `🎉 ¡Ganaste! ¡Felicidades!`;
  }
}

// ==========================
// 🔄 REINICIAR JUEGO
// ==========================
function reiniciarJuego() {
  iniciarJuego();
}

// ==========================
// 📋 REGLAS
// ==========================
const reglasTexto = computed(() => {
  const nivel = localStorage.getItem("nivelSeleccionado");
  switch (nivel) {
    case "facil": return "14 intentos - Palabras cortas y comunes";
    case "medio": return "10 intentos - Palabras de dificultad media";
    case "experto": return "9 intentos - Palabras largas y complejas";
    default: return "Selecciona un nivel";
  }
});
</script>

<style scoped>
.juego-container {
  max-width: 600px;
  margin: auto;
  text-align: center;
}

.ahorcado .dibujo {
  font-family: monospace;
  white-space: pre;
  margin-bottom: 20px;
  line-height: 1.2;
}

.teclado button {
  margin: 2px;
  padding: 10px;
  font-size: 16px;
  cursor: pointer;
}

.teclado button.correcta {
  background-color: #4caf50;
  color: white;
}

.teclado button.incorrecta {
  background-color: #f44336;
  color: white;
}

.teclado button.disabled {
  cursor: not-allowed;
  opacity: 0.5;
}

.letra-casilla {
  display: inline-block;
  width: 25px;
  font-size: 24px;
  border-bottom: 2px solid black;
  margin: 0 2px;
}
</style>






<script setup>
import { ref, onMounted, computed, onUnmounted } from 'vue';

const dibujos = [
`
`,
`
|
|
|
|
|
`,
`
+---+
|
|
|
|
|
`,
`
+---+
|   O
|
|
|
|
`,
`
+---+
|   O
|   |
|
|
|
`,
`
+---+
|   O
|  /|
|
|
|
`,
`
+---+
|   O
|  /|\\
|
|
|
`,
`
+---+
|   O
|  /|\\
|  /
|
|
`,
`
+---+
|   O
|  /|\\
|  / \\
|
|
`
];

const dibujoAhorcado = ref(dibujos[0]);





const teclado = ref(
  "abcdefghijklmnopqrstuvwxyz".split("")
);

const palabraAleatoria = ref("");
const letrasEncontradas = ref([]);
const letrasUsadas = ref([]);
const letrasCorrectas = ref([]);
const letrasIncorrectas = ref([]);

const intentosRestantes = ref(0);
const juegoTerminado = ref(false);
const mensaje = ref("");

// Convertir dibujo a HTML con <br>
const dibujoHTML = computed(() => {
  return dibujoAhorcado.value.replace(/\n/g, "<br>");
});

<template>
  <div class="juego-container">


<div class="score">

<q-btn style="background: goldenrod; color: white" label="score" 
to="/score"/>
</div>

    <!-- DIBUJO AHORCADO -->
    <div class="ahorcado">
      <div class="dibujo" v-html="dibujoHTML"></div>
    </div>

    <!-- INTENTOS RESTANTES -->
    <div class="info-juego">
      <div class="intentos">
        <span class="emoji">❤️</span>
        <span class="contador">{{ intentosRestantes }}</span>
        <span class="texto">Intentos</span>
      </div>
    </div>

    <!-- PALABRA -->
    <div class="palabra-container">
      <p class="palabra">
        <span v-for="(letra, i) in letrasEncontradas" :key="i" class="letra-casilla">
          {{ letra }}
        </span>
      </p>
      <p class="mensaje">{{ mensaje }}</p>
    </div>

    <!-- TECLADO -->
    <div class="teclado">
      <button 
        v-for="(letra, i) in teclado" 
        :key="i" 
        @click="presionarLetra(letra)"
        :disabled="letrasUsadas.includes(letra) || juegoTerminado"
        :class="{
          'correcta': letrasCorrectas.includes(letra),
          'incorrecta': letrasIncorrectas.includes(letra),
          'disabled': letrasUsadas.includes(letra)
        }"
      >
        {{ letra }}
      </button>
    </div>

    <!-- BOTONES DE ACCIÓN -->
    <div class="botones-accion">
      <button @click="reiniciarJuego" class="btn-reiniciar">🔄 Reiniciar</button>
      <button @click="$router.push('/')" class="btn-inicio">🏠 Volver a Inicio</button>
    </div>

    <!-- REGLAS -->
    <div class="reglas-nivel">
      <h3>📋 Reglas del Nivel:</h3>
      <p>{{ reglasTexto }}</p>
    </div>

   
    <div v-if="contadorIniciado && !juegoTerminado" class="tiempo-actual">
      <p>⏱️ Tiempo: {{ tiempo }} segundos</p>
    </div>
    
    <!-- CONTADOR DE TIEMPO CUANDO GANA -->
    <div v-if="juegoTerminado && tiempoGanado > 0" class="tiempo-ganado">
      <p>🏆 ¡Ganaste en {{ tiempoGanado }} segundos!</p>
    </div>

  </div>
</template>


<script setup>
import { ref, onMounted, computed, onUnmounted } from 'vue';

const dibujos = [
`
`,
`
|
|
|
|
|
`,
`
+---+
|
|
|
|
|
`,
`
+---+
|   O
|
|
|
|
`,
`
+---+
|   O
|   |
|
|
|
`,
`
+---+
|   O
|  /|
|
|
|
`,
`
+---+
|   O
|  /|\\
|
|
|
`,
`
+---+
|   O
|  /|\\
|  /
|
|
`,
`
+---+
|   O
|  /|\\
|  / \\
|
|
`
];

const dibujoAhorcado = ref(dibujos[0]);
const teclado = ref("abcdefghijklmnopqrstuvwxyz".split(""));

const palabraAleatoria = ref("");
const letrasEncontradas = ref([]);
const letrasUsadas = ref([]);
const letrasCorrectas = ref([]);
const letrasIncorrectas = ref([]);

const intentosRestantes = ref(0);
const juegoTerminado = ref(false);
const mensaje = ref("");
const tiempo = ref(0);
const tiempoGanado = ref(0);
const contadorIniciado = ref(false); // Nuevo: para controlar si el contador ya inició
let startTime;
let timer;

// Convertir dibujo a HTML con <br>
const dibujoHTML = computed(() => {
  return dibujoAhorcado.value.replace(/\n/g, "<br>");
});

// Función para iniciar el contador
function iniciarContador() {
  if (contadorIniciado.value) return; // No iniciar si ya está corriendo
  
  contadorIniciado.value = true;
  startTime = Date.now();
  timer = setInterval(() => {
    tiempo.value = Math.floor((Date.now() - startTime) / 1000);
  }, 1000);
  console.log("Contador iniciado");
}

// Función para detener el contador
function detenerContador() {
  if (timer) {
    clearInterval(timer);
    timer = null;
    console.log("Contador detenido");
  }
}

// Guardar tiempo cuando gana
function guardarTiempoGanado() {
  tiempoGanado.value = tiempo.value;
  detenerContador();
}

function iniciarJuego() {
  const nivel = localStorage.getItem("nivelSeleccionado") || "facil";
  const palabrasCategoria = localStorage.getItem("palabrasCategoria");

  // Detener cualquier contador previo
  detenerContador();
  
  // Resetear variables
  letrasUsadas.value = [];
  letrasCorrectas.value = [];
  letrasIncorrectas.value = [];
  dibujoAhorcado.value = dibujos[0];
  juegoTerminado.value = false;
  mensaje.value = "";
  tiempo.value = 0;
  tiempoGanado.value = 0;
  contadorIniciado.value = false; // Resetear bandera del contador

  if (!palabrasCategoria) {
    mensaje.value = "❌ No hay palabras disponibles. Regresa al inicio.";
    return;
  }

  let palabras = [];
  try {
    palabras = JSON.parse(palabrasCategoria);
  } catch (e) {
    console.error("Error al convertir JSON:", e);
    mensaje.value = "❌ Error al cargar las palabras.";
    return;
  }

  if (!Array.isArray(palabras) || palabras.length === 0) {
    mensaje.value = "❌ No hay palabras en esta categoría.";
    return;
  }

  palabraAleatoria.value = palabras[Math.floor(Math.random() * palabras.length)];
  letrasEncontradas.value = palabraAleatoria.value.split("").map(() => "_");

  // Configurar intentos según nivel
  if (nivel === "facil") {
    intentosRestantes.value = 14;
  } else if (nivel === "medio") {
    intentosRestantes.value = 10;
  } else {
    intentosRestantes.value = 9;
  }
  
  console.log("Juego iniciado. Palabra:", palabraAleatoria.value);
}

function presionarLetra(letra) {
  if (juegoTerminado.value || letrasUsadas.value.includes(letra)) return;
  
  // Si es la primera letra presionada, iniciar contador
  if (letrasUsadas.value.length === 0) {
    iniciarContador();
  }
  
  letrasUsadas.value.push(letra);

  if (!palabraAleatoria.value.toLowerCase().includes(letra)) {
    letrasIncorrectas.value.push(letra);
    intentosRestantes.value--;
    
    // Actualizar dibujo del ahorcado
    const indiceDibujo = Math.min(letrasIncorrectas.value.length, dibujos.length - 1);
    dibujoAhorcado.value = dibujos[indiceDibujo];
    
    mensaje.value = `❌ Letra "${letra}" incorrecta. Te quedan ${intentosRestantes.value} intentos`;

    if (intentosRestantes.value <= 0) {
      juegoTerminado.value = true;
      mensaje.value = `💀 ¡Perdiste! La palabra era: "${palabraAleatoria.value}"`;
      detenerContador(); // Detener contador si pierde
    }
    return;
  }

  // Letra correcta
  letrasCorrectas.value.push(letra);
  mensaje.value = `✅ ¡Correcto! Letra "${letra}" encontrada`;

  // Revelar letra en la palabra
  for (let i = 0; i < palabraAleatoria.value.length; i++) {
    if (palabraAleatoria.value[i].toLowerCase() === letra) {
      letrasEncontradas.value[i] = palabraAleatoria.value[i];
    }
  }

  // Verificar si ganó
  if (!letrasEncontradas.value.includes("_")) {
    juegoTerminado.value = true;
    mensaje.value = `🎉 ¡Ganaste! ¡Felicidades!`;
    guardarTiempoGanado(); // Detener y guardar tiempo
  }
}

function reiniciarJuego() {
  iniciarJuego();
}

const reglasTexto = computed(() => {
  const nivel = localStorage.getItem("nivelSeleccionado") || "facil";
  if (nivel === "facil") {
    return "14 intentos - Palabras cortas y comunes";
  } else if (nivel === "medio") {
    return "10 intentos - Palabras de dificultad media";
  } else {
    return "9 intentos - Palabras largas y complejas";
  }
});

// Iniciar juego al montar el componente
onMounted(() => {
  iniciarJuego();
});

// Detener contador cuando el componente se desmonta
onUnmounted(() => {
  detenerContador();
});
console.log("Letra presionada:", letra);
console.log("Contador iniciado:", contadorIniciado.value);
console.log("Tiempo actual:", tiempo.value);
</script>

<style >
.juego-container {
  max-width: 600px;
  margin: auto;
  text-align: center;
}

.ahorcado .dibujo {
  font-family: monospace;
  white-space: pre;
  line-height: 1.2;
  margin-bottom: 20px;
}

.teclado button {
  margin: 2px;
  padding: 10px;
  font-size: 16px;
  cursor: pointer;
}

.teclado button.correcta {
  background-color: #4caf50;
  color: white;
}

.teclado button.incorrecta {
  background-color: #f44336;
  color: white;
}

.teclado button.disabled {
  cursor: not-allowed;
  opacity: 0.5;
}

.letra-casilla {
  display: inline-block;
  width: 25px;
  font-size: 24px;
  border-bottom: 2px solid black;
  margin: 0 2px;
}

.tiempo-ganado {
  background-color: #e8f5e9;
  border: 2px solid #4caf50;
  border-radius: 10px;
  padding: 15px;
  margin: 20px 0;
  color: #2e7d32;
  font-weight: bold;
  animation: pulse 1.5s infinite;
}

@keyframes pulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.05); }
  100% { transform: scale(1); }
}
</style>



const tiempo = ref(0);
const tiempoGanado = ref(0); // Nuevo: almacena el tiempo cuando gana
let startTime;
let timer;

// Función para iniciar el contador (solo llamar cuando presiona la primera letra)
function iniciarContador() {
  startTime = Date.now();
  timer = setInterval(() => {
    tiempo.value = Math.floor((Date.now() - startTime) / 1000);
  }, 1000);
}

// Función para detener el contador
function detenerContador() {
  clearInterval(timer);
}

// Guardar tiempo cuando gana
function guardarTiempoGanado() {
  tiempoGanado.value = tiempo.value;
  detenerContador();
}

function iniciarJuego() {
  // ... código anterior ...
  
  // Resetear tiempos
  tiempo.value = 0;
  tiempoGanado.value = 0;
  
  // ... resto del código ...
}

function presionarLetra(letra) {
  // Si es la primera letra presionada, iniciar contador
  if (letrasUsadas.value.length === 0) {
    iniciarContador();
  }
  
  if (juegoTerminado.value || letrasUsadas.value.includes(letra)) return;
  
  letrasUsadas.value.push(letra);
  
  if (!palabraAleatoria.value.toLowerCase().includes(letra)) {
    // ... código para letra incorrecta ...
  } else {
    letrasCorrectas.value.push(letra);
    mensaje.value = `✅ ¡Correcto! Letra "${letra}" encontrada`;
    
    for (let i = 0; i < palabraAleatoria.value.length; i++) {
      if (palabraAleatoria.value[i].toLowerCase() === letra) {
        letrasEncontradas.value[i] = letra;
      }
    }
    
    if (!letrasEncontradas.value.includes("_")) {
      juegoTerminado.value = true;
      mensaje.value = `🎉 ¡Ganaste! ¡Felicidades!`;
      guardarTiempoGanado(); // Detener y guardar tiempo
    }
  }
}

// Detener contador cuando el componente se desmonta
onUnmounted(() => {
  if (timer) {
    detenerContador();
  }
});


function iniciarJuego() {
  const nivel = localStorage.getItem("nivelSeleccionado") || "facil";
  const palabrasCategoria = localStorage.getItem("palabrasCategoria");

  letrasUsadas.value = [];
  letrasCorrectas.value = [];
  letrasIncorrectas.value = [];
  dibujoAhorcado.value = dibujos[0];
  juegoTerminado.value = false;
  mensaje.value = "";

  if (!palabrasCategoria) return;

  let palabras = [];
  try {
    palabras = JSON.parse(palabrasCategoria);
  } catch (e) {
    console.error("Error al convertir JSON:", e);
    return;
  }

  if (!Array.isArray(palabras) || palabras.length === 0) return;

  palabraAleatoria.value = palabras[Math.floor(Math.random() * palabras.length)];
  letrasEncontradas.value = palabraAleatoria.value.split("").map(() => "_");

  intentosRestantes.value = nivel === "facil" ? 14 : nivel === "medio" ? 10 : 9;
}

onMounted(() => iniciarJuego());

function presionarLetra(letra) {
  if (juegoTerminado.value || letrasUsadas.value.includes(letra)) return;
onMounted(() => {
  iniciarContador();
});
  letrasUsadas.value.push(letra);

  if (!palabraAleatoria.value.toLowerCase().includes(letra)) {
    letrasIncorrectas.value.push(letra);
    intentosRestantes.value--;
    dibujoAhorcado.value = dibujos[Math.min(letrasIncorrectas.value.length, dibujos.length - 1)];
    mensaje.value = `❌ Letra "${letra}" incorrecta. Te quedan ${intentosRestantes.value} intentos`;

    if (intentosRestantes.value <= 0) {
      juegoTerminado.value = true;
      mensaje.value = `💀 ¡Perdiste! La palabra era: "${palabraAleatoria.value}"`;
      onUnmounted(() => {
  detenerContador();
});

    }
    return;
  }

  letrasCorrectas.value.push(letra);
  mensaje.value = `✅ ¡Correcto! Letra "${letra}" encontrada`;

  for (let i = 0; i < palabraAleatoria.value.length; i++) {
    if (palabraAleatoria.value[i].toLowerCase() === letra) {
      letrasEncontradas.value[i] = letra;
    }
  }

  if (!letrasEncontradas.value.includes("_")) {
    juegoTerminado.value = true;
    mensaje.value = `🎉 ¡Ganaste! ¡Felicidades!`;


  }
}

function reiniciarJuego() {
  iniciarJuego();
}

const reglasTexto = computed(() => {
  const nivel = localStorage.getItem("nivelSeleccionado") || "facil";
  return nivel === "facil"
    ? "14 intentos - Palabras cortas y comunes"
    : nivel === "medio"
    ? "10 intentos - Palabras de dificultad media"
    : "9 intentos - Palabras largas y complejas";
});
</script>