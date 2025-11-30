<!-- <template>
    <div>
        <div class="tittle">
<h1>Selecciona una categoria para competir</h1>
        </div>

        <div class="contenedor">

<div class="hijo1">
<div class="frutas">
    <img src="https://cdn.pixabay.com/photo/2016/10/30/18/01/apple-1783882_640.png" alt="">
    <h3>Frutas</h3>
</div>
<div class="animales">
    <img src="https://symbl-cdn.com/i/webp/fd/a1842713acac725db137779e71373e.webp" alt="">
    <h3>Animales</h3>
</div>
<div class="paises">
    <img src="https://e1.pngegg.com/pngimages/244/611/png-clipart-emojis-planet-earth-illustration-thumbnail.png" alt="">
    <h3>Paises</h3>
</div>
</div>

<div class="hijo2">
<div class="deportes">
    <img src="https://img.pikbest.com/origin/10/12/62/99YpIkbEsTB2V.jpg!w700wp" alt="">
    <h3>Deportes</h3>
</div>
<div class="peliculas">
    <img src="https://images.emojiterra.com/openmoji/v13.1/512px/1f3ac.png" alt="">
        <h3>Peliculas</h3>
</div>
<div class="ciencia">
    <img src="https://em-content.zobj.net/source/whatsapp/238/telescope_1f52d.png" alt="">
    <h3>Ciencia</h3>
</div>

</div>

        </div>
    </div>
</template>






<script setup>
const eleccion = ref()

</script>


<style>


.contenedor{
    display: grid;
    grid-template-rows: repeat(2, 1fr);

}

.hijo1{
    display: flex;
    flex-direction: row;
     gap: 40px;
}
.hijo2{
    display: flex;
    flex-direction: row;
    gap: 40px;
}

.frutas img{
width: 60px;
height: auto;
}

</style> -->



<template>
    <div class="categorias-container">
        <div class="titulo">
            <h1>Selecciona una categoría para competir</h1>
            <p>Elige tu tema favorito y demuestra tus conocimientos</p>
        </div>

        <div class="categorias-grid">
            <div class="categoria-card" @click="seleccionarCategoria('frutas')">
                <div class="categoria-imagen frutas-bg">
                    <img src="https://cdn.pixabay.com/photo/2016/10/30/18/01/apple-1783882_640.png" alt="Frutas">
                </div>
                <div class="categoria-info">
                    <h3>Frutas</h3>
                    <p>¿Conoces todas las frutas?</p>
                </div>
            </div>

            <div class="categoria-card" @click="seleccionarCategoria('animales')">
                <div class="categoria-imagen animales-bg">
                    <img src="https://symbl-cdn.com/i/webp/fd/a1842713acac725db137779e71373e.webp" alt="Animales">
                </div>
                <div class="categoria-info">
                    <h3>Animales</h3>
                    <p>El reino animal te espera</p>
                </div>
            </div>

            <div class="categoria-card" @click="seleccionarCategoria('paises')">
                <div class="categoria-imagen paises-bg">
                    <img src="https://e1.pngegg.com/pngimages/244/611/png-clipart-emojis-planet-earth-illustration-thumbnail.png" alt="Países">
                </div>
                <div class="categoria-info">
                    <h3>Países</h3>
                    <p>Viaja por el mundo</p>
                </div>
            </div>

            <div class="categoria-card" @click="seleccionarCategoria('deportes')">
                <div class="categoria-imagen deportes-bg">
                    <img src="https://img.pikbest.com/origin/10/12/62/99YpIkbEsTB2V.jpg!w700wp" alt="Deportes">
                </div>
                <div class="categoria-info">
                    <h3>Deportes</h3>
                    <p>Demuestra tu conocimiento deportivo</p>
                </div>
            </div>

            <div class="categoria-card" @click="seleccionarCategoria('peliculas')">
                <div class="categoria-imagen peliculas-bg">
                    <img src="https://images.emojiterra.com/openmoji/v13.1/512px/1f3ac.png" alt="Películas">
                </div>
                <div class="categoria-info">
                    <h3>Películas</h3>
                    <p>El mundo del cine</p>
                </div>
            </div>

            <div class="categoria-card" @click="seleccionarCategoria('ciencia')">
                <div class="categoria-imagen ciencia-bg">
                    <img src="https://em-content.zobj.net/source/whatsapp/238/telescope_1f52d.png" alt="Ciencia">
                </div>
                <div class="categoria-info">
                    <h3 >Ciencia</h3>
                    <p>Explora el conocimiento científico</p>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
function seleccionarCategoria(categoria) {
  localStorage.setItem
('categoriaSeleccionada', categoria)
console.log("guardado en el local storage")
}

</script>

<style scoped>
.categorias-container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px;
    font-family: 'Arial', sans-serif;
}

.titulo {
    text-align: center;
    margin-bottom: 40px;
}

.titulo h1 {
    color: #333;
    font-size: 2.5rem;
    margin-bottom: 10px;
}

.titulo p {
    color: #666;
    font-size: 1.2rem;
}

.categorias-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 25px;
}

.categoria-card {
    background: white;
    border-radius: 15px;
    overflow: hidden;
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
    transition: all 0.3s ease;
    cursor: pointer;
}

.categoria-card:hover {
    transform: translateY(-10px);
    box-shadow: 0 15px 30px rgba(0, 0, 0, 0.15);
}

.categoria-imagen {
    height: 180px;
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;
    overflow: hidden;
}

.categoria-imagen::after {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.2);
    z-index: 1;
}

.categoria-imagen img {
    width: 80px;
    height: 80px;
    object-fit: contain;
    z-index: 2;
    filter: drop-shadow(0 2px 5px rgba(0, 0, 0, 0.3));
}

/* Fondos personalizados para cada categoría */
.frutas-bg {
    background: linear-gradient(135deg, #ff9a9e 0%, #fad0c4 100%);
}

.animales-bg {
    background: linear-gradient(135deg, #a1c4fd 0%, #c2e9fb 100%);
}

.paises-bg {
    background: linear-gradient(135deg, #ffecd2 0%, #fcb69f 100%);
}

.deportes-bg {
    background: linear-gradient(135deg, #84fab0 0%, #8fd3f4 100%);
}

.peliculas-bg {
    background: linear-gradient(135deg, #d4fc79 0%, #96e6a1 100%);
}

.ciencia-bg {
    background: linear-gradient(135deg, #a6c0fe 0%, #f68084 100%);
}

.categoria-info {
    padding: 20px;
    text-align: center;
}

.categoria-info h3 {
    margin: 0 0 8px 0;
    color: #333;
    font-size: 1.5rem;
}

.categoria-info p {
    margin: 0;
    color: #666;
    font-size: 0.9rem;
}

/* Responsive */
@media (max-width: 768px) {
    .categorias-grid {
        grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
        gap: 20px;
    }
    
    .titulo h1 {
        font-size: 2rem;
    }
    
    .categoria-imagen {
        height: 150px;
    }
}

@media (max-width: 480px) {
    .categorias-grid {
        grid-template-columns: 1fr;
    }
    
    .titulo h1 {
        font-size: 1.8rem;
    }
    
    .categorias-container {
        padding: 10px;
    }
}
</style>