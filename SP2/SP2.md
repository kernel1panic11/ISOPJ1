---
layout: default
title: "Sprint 2. Instalación, configuración de software base y gestión de ficheros."
---

<main class="contenedor-principal">
    <h1 class="titulo">Sprint 2. Instalación, configuración de software base y gestión de ficheros.</h1> 
    <div class="loading-bar"><div class="loading-progress"></div></div> 
    <div class="navigation-links">
        <a href="{{ '/' | relative_url }}"><i class="fa-solid fa-house"></i> Volver al Inicio</a>
        <a href="{{ '/SP3/SP3.html' | relative_url }}">Siguiente Práctica <i class="fa-solid fa-arrow-right"></i></a>
    </div>
    <div class="content-section">
        <h2 class="sub">Sistema de ficheros y particiones:</h2>
        <ul>
            <li>Tamaño del sector</li>
            <li>Tamaño del bloque</li>
            <li>Fragmentación interna</li>
            <li>Fragmentación externa</li>
            <li>Tipos de formateo
                <ul>
                    <li>&gt;&gt;Bajo nivel</li>
                    <li>&gt;&gt;Medio nivel</li>
                    <li>&gt;&gt;Alto nivel</li>
                </ul>
            </li>
        </ul>
    </div>
    <div class="content-section">
        <h2 class="sub">Gestión de particiones:</h2>
        <ul>
            <li>Gparted</li>
            <li>Comandos</li>
        </ul>
    </div>
    Disco vista vmware
    <img width="826" height="670" alt="image" src="https://github.com/user-attachments/assets/d2fce307-de10-4ac3-80e6-8d1ea669cd9e" />
    Medida del sector 
    <img width="933" height="569" alt="image" src="https://github.com/user-attachments/assets/ff7b4574-eb1e-48bc-a71a-11da2425f37f" />
    Medida del bloque 
    <img width="621" height="128" alt="image" src="https://github.com/user-attachments/assets/a3a3b284-07db-4318-9f51-bd2eb2005c3d" />
    Ver sistema de archivos
    <img width="699" height="180" alt="image" src="https://github.com/user-attachments/assets/ca0866b5-6e8f-4668-b300-59c24d8ac9a7" />
    medida del bloque 
    <img width="682" height="191" alt="image" src="https://github.com/user-attachments/assets/04eca4eb-684d-45af-8bc4-ec36fbfd739c" />
    Particones: una particion, es un trozo fisico del disco duro, pero no podemos modificar la medida del bloque, eso se hace pero terminal 
    El gparted sirve para gestionar paticiones
    <img width="777" height="532" alt="image" src="https://github.com/user-attachments/assets/53f4f163-3724-4527-8ade-8cdf99407de5" />





    
    <div class="content-section">
        <h2 class="sub">Gestión de procesos</h2>
    </div>
    <div class="content-section">
        <h2 class="sub">Gestión de usuarios, grupos y permisos</h2>
         <ul>
            <li>&gt;&gt;Copias de seguridad y automatización de tareas</li>
            <li>&gt;&gt;Cuotas de usuario</li>
        </ul>
    </div>
    </main>

<button id="scrollToTopBtn" class="scroll-btn" title="Ir arriba"><i class="fa-solid fa-arrow-up"></i></button>
<button id="scrollToBottomBtn" class="scroll-btn" title="Ir abajo"><i class="fa-solid fa-arrow-down"></i></button>

<style>
:root {
    /* Esto anula el fondo por defecto y pone el de pract2.gif */
    --bg-image: url('../assetscss/pract2.gif');
}

.navigation-links {
    /* --- HACE QUE LA BARRA SEA DINÁMICA (STICKY) --- */
    position: sticky;
    top: 1rem; 
    z-index: 100;
    /* --- FIN DE LA PARTE STICKY --- */

    background: var(--overlay);
    backdrop-filter: blur(8px);
    border: 1px solid var(--border);
    border-radius: var(--radius); 
    box-shadow: var(--shadow-neon);
    margin-top: 1.5rem; 
    margin-bottom: 1.5rem;
    padding: 1rem 1.2rem; 
    border-top: 1px solid var(--border); 
}

main.contenedor-principal {
    padding-bottom: 2.5rem; 
}

/* --- ESTILOS PARA LOS BOTONES DE SCROLL --- */
.scroll-btn {
  position: fixed;
  bottom: 20px;
  width: 50px;
  height: 50px;
  background: var(--overlay);
  color: var(--accent);
  border: 1px solid var(--border);
  border-radius: 50%;
  cursor: pointer;
  font-size: 1.5rem;
  z-index: 1000;
  backdrop-filter: blur(5px);
  box-shadow: var(--shadow-neon);
  transition: transform 0.2s ease, box-shadow 0.2s ease, background-color 0.2s ease;
}
.scroll-btn:hover {
  transform: scale(1.1);
  box-shadow: 0 0 25px rgba(0, 255, 255, 0.75);
  background: rgba(0, 255, 255, 0.1);
}
#scrollToTopBtn {
  right: 20px;
}
#scrollToBottomBtn {
  left: 20px;
}
/* --- FIN DE ESTILOS DE BOTONES --- */
</style>
