<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue';

const envoltura = ref(null);
const carta = ref(null);

const handleClick = (e) => {
  if (
    e.target.matches(".sobre") ||
    e.target.matches(".solapa-derecha") ||
    e.target.matches(".solapa-izquierda") ||
    e.target.matches(".corazon")
  ) {
    envoltura.value.classList.toggle("abierto");
  } else if (e.target.matches(".sobre *")) {
    if (!carta.value.classList.contains("abierta")) {
      carta.value.classList.add("mostrar-carta");
      setTimeout(() => {
        carta.value.classList.remove("mostrar-carta");
        carta.value.classList.add("abierta");
      }, 500);
      envoltura.value.classList.add("desactivar-sobre");
    } else {
      carta.value.classList.add("cerrando-carta");
      envoltura.value.classList.remove("desactivar-sobre");
      setTimeout(() => {
        carta.value.classList.remove("cerrando-carta");
        carta.value.classList.remove("abierta");
      }, 500);
    }
  }
};

onMounted(() => {
  document.addEventListener('click', handleClick);
});

onBeforeUnmount(() => {
  document.removeEventListener('click', handleClick);
});
</script>

<template>

  <div id="app">
    <section>
      <h1>¡Abre la carta UwU! ❤️</h1>
      <div class="contenedor">
        <div class="envoltura-sobre" ref="envoltura">
          <div class="sobre">
            <div class="carta" ref="carta">
              <div class="contenido">
                <p>
                  Hola mi amor, sabes siempre te he querido hacerte cosas relacionadas a mi carrera,
                  pero no he tenido el tiempo de hacerlo hasta ahora. Feliz 14 de febrero mi amor,
                  espero que te guste este pequeño detalle, sabes que te amo mucho preciosa.
                </p>
                <img src="" alt="">
              </div>
            </div>
          </div>
          <div class="corazon"></div>
          <div class="solapa-derecha"></div>
          <div class="solapa-izquierda"></div>
        </div>
      </div>
    </section>
  </div>
</template>

<style>


* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

:root {
    --primario: #fff;
    --fondo: #ffebf2;
    --fondo-sobre: #ffe3ed;
    --solapa-sobre: #ffccd5;
    --cuerpo-sobre: #ffc1d1;
    --sombra: rgba(0, 0, 0, 0.2);
    --texto: #003049;
    --corazon: #ff477e;
}

body {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    height: 100vh;
}

h1 {
    text-align: center;
}

.contenedor {
    width: 100%;
}

.envoltura-sobre {
    position: relative;
    background-color: var(--fondo-sobre);
    box-shadow: 0 0 40px var(--sombra);
}

.sobre {
    position: relative;
    width: 400px;
    height: 300px;
}

.sobre::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    z-index: 4;
    border-top: 180px solid var(--solapa-sobre);
    border-right: 200px solid transparent;
    border-left: 200px solid transparent;
    transform-origin: top;
    transition: all 0.5s ease-in-out 0.5s;
    border-radius: 10px;

}

/* parte del sobre del lado derecho */
.solapa-derecha {
    position: absolute;
    top: 0;
    right: 0;
    width: 100%;
    height: 100%;
    clip-path: polygon(100% 0, 0 100%, 100% 100%);
    background-color: var(--cuerpo-sobre);
    border-bottom-right-radius: 10px;
    border-top-right-radius: 10px;
    border-bottom-left-radius: 10px;
}

/* parte del sobre del lado izquierdo */
.solapa-izquierda {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    clip-path: polygon(0 0, 0 100%, 100% 100%);
    background-color: var(--cuerpo-sobre);
    border-top-left-radius: 10px;
    border-bottom-left-radius: 10px;
    border-bottom-right-radius: 10px;

}

.carta {
    position: absolute;
    bottom: 0;

    width: 100%;
    height: 100%;

    background-color: var(--primario);
    text-align: center;
    padding: 20px;
    border-radius: 10px;
    transition: transform 0.3s ease-in-out;
    cursor: pointer;
}

/*estilo a todo el contenido de la carte */
.contenido {
    text-align: left;
    font-size: 16px;
    padding: 10px;
    line-height: 20px;
    cursor: pointer;
}

/* estilo del corazón de la carta */
.corazon {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 30px;
    height: 30px;
    background-color: var(--corazon);
    transform: translate(-50%,0) rotate(45deg);
    transition: transform 0.5s ease-in-out 1s;
    z-index: 999;
    cursor: pointer;
}

.corazon::before,
.corazon::after {
    content: "";
    position: absolute;
    width: 30px;
    height: 30px;
    background-color: var(--corazon);
    border-radius: 100%;
}

.corazon:before {
    top: -15px;
}

.corazon:after {
    right: 15px;
}

/* Clases dinámicas */
.abierto .sobre::before {
    transform: rotateX(180deg);
    z-index: 0;
}

.abierto .corazon {
    transform: rotate(90deg);
    transition-delay: 0.4s;
}

.carta.mostrar-carta {
    transform: translateY(-290px);
    transition: transform .5s ease-in-out;
}

.carta.cerrando-carta {
    transform: translateY(-290px);
    transition: transform .5s ease-in-out;
}

.carta.abierta {
    z-index: 10000;
}

.envoltura-sobre.desactivar-sobre .sobre::before {
    pointer-events: none;
}


@media screen and (max-width:400px) {
    .contenedor {
        width: 300px;
    }
    .sobre {
        width: 300px;
        height: 250px;
    }

    .sobre::before {
        border-top:150px solid var(--solapa-sobre) ;
        border-right: 150px solid transparent;
        border-left: 150px solid transparent;
    }
}
</style>
