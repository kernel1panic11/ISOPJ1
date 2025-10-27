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
    <img width="826" height="670" alt="image" src="https://github.com/user-attachments/assets/d2fce307-de10-4ac3-80e6-8d1ea669cd9e" class="course-image"/>
    Medida del sector 
    <img width="933" height="569" alt="image" src="https://github.com/user-attachments/assets/ff7b4574-eb1e-48bc-a71a-11da2425f37f" class="course-image"/>
    Medida del bloque 
    <img width="621" height="128" alt="image" src="https://github.com/user-attachments/assets/a3a3b284-07db-4318-9f51-bd2eb2005c3d" class="course-image"/>
    Ver sistema de archivos
    <img width="699" height="180" alt="image" src="https://github.com/user-attachments/assets/ca0866b5-6e8f-4668-b300-59c24d8ac9a7" class="course-image"/>
    medida del bloque 
    <img width="682" height="191" alt="image" src="https://github.com/user-attachments/assets/04eca4eb-684d-45af-8bc4-ec36fbfd739c" class="course-image"/>
    Particones: una particion, es un trozo fisico del disco duro, pero no podemos modificar la medida del bloque, eso se hace pero terminal, un volumen es una capa de abstracion que se pone encima de las particiones y/o discos. 
    El gparted sirve para gestionar paticiones
    <img width="777" height="532" alt="image" src="https://github.com/user-attachments/assets/53f4f163-3724-4527-8ade-8cdf99407de5" class="course-image"/>
    particiones terminal
    <img width="931" height="761" alt="image" src="https://github.com/user-attachments/assets/d17a2000-1cb1-48ef-9594-029f21bfeba1" class="course-image"/>
    Primera particion ext4
    <img width="893" height="294" alt="image" src="https://github.com/user-attachments/assets/8f70f76c-b256-4adf-b9ed-ca11e9f471be" class="course-image"/>
    Formatear segunda particion en nfts: 
    <img width="563" height="134" alt="image" src="https://github.com/user-attachments/assets/9ffc4cf6-337d-4a5a-b2e7-be66dabf4a9e" class="course-image"/>
    Comprobando por terminal 
    <img width="1039" height="201" alt="image" src="https://github.com/user-attachments/assets/e6fbdcd9-fe1a-4280-b15b-b4c410f46e85" class="course-image"/>
    Comprobando con gparted
    <img width="771" height="257" alt="image" src="https://github.com/user-attachments/assets/10f5d46d-f9bc-47cc-bfe4-5b89e0a004dc" class="course-image"/>
    Montar particion modo temporal
    <img width="624" height="144" alt="image" src="https://github.com/user-attachments/assets/56fe0548-4eb7-4d4f-a550-8653953e5818" class="course-image"/>
    <img width="338" height="31" alt="image" src="https://github.com/user-attachments/assets/8cabda37-5690-4f65-8d4a-57bae83564c5" class="course-image"/>
    Montar particion de modo permanente:
    <img width="808" height="256" alt="image" src="https://github.com/user-attachments/assets/93f4df08-d910-4f46-9f6b-04daf5424912" class="course-image"/>
    Comprobacion despues de reinicio
    <img width="820" height="179" alt="image" src="https://github.com/user-attachments/assets/732511da-07a3-4228-ac73-ee2b77ba7ca0" class="course-image"/>
    Desfragmentacion
    <img width="1288" height="514" alt="image" src="https://github.com/user-attachments/assets/f9e8f8d2-03d9-4d99-8342-e5669b44f21f" class="course-image"/>



    


    





    
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
