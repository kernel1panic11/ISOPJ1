---
layout: default
title: "Sprint 3. Administracion de dominios y seguridad"
---

<main class="contenedor-principal">
    <h1 class="titulo">Sprint 3. Administracion de dominios y seguridad</h1>
    <div class="loading-bar"><div class="loading-progress"></div></div>
    <div class="navigation-links">
        <a href="{{ '/' | relative_url }}"><i class="fa-solid fa-house"></i> Volver al inicio</a>
        <a href="{{ '/SP3/SP3.html' | relative_url }}">Siguiente práctica <i class="fa-solid fa-arrow-right"></i></a>
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
                    <li><strong>Bajo nivel</strong>: lo que hace es borrar todo; sobrescribe todo con ceros e intenta reparar los sectores defectuosos, pero se necesitan programas específicos; no se puede hacer a través del sistema operativo de forma normal.</li>
                    <li><strong>Medio nivel</strong>: sería un intermedio, no borra archivos; los sectores defectuosos que encuentra los marca, pero no intenta arreglarlos. También se pueden recuperar los archivos.</li>
                    <li><strong>Alto nivel</strong>: no borra los archivos, solo borra el sistema de archivos (la tabla de asignación) e ignora los sectores defectuosos que pueda encontrar.</li>
                </ul>
            </li>
        </ul>
        <p></p>
    </div>
     <h2 class="sub">LDAP</h2>
<p>Empezamos editando el hostname, con este nombre sera con el que sera reconozido en la red </p>
<img width="1898" height="161" alt="image" src="https://github.com/user-attachments/assets/e4916ad7-1898-4624-a235-fd7b80b40e9d" class="course-image"/>
<p>Ahora editamos el /etc/host </p>
<img width="1395" height="333" alt="image" src="https://github.com/user-attachments/assets/d4d3b772-7efb-4a4e-acf4-e6736d01be92" class="course-image"/>
<p>Despues de esto, lo que hacemos es instalarle el ldap al ubuntu que hara de servidor </p>
<img width="881" height="385" alt="image" src="https://github.com/user-attachments/assets/6a68e5fc-bd14-4449-8091-d095cb3796f0" class="course-image"/>
<img width="1919" height="1029" alt="image" src="https://github.com/user-attachments/assets/33ad9380-1df9-4ec8-8043-41dceb1f5e45" class="course-image"/>
<p>Despues de hacer esta configuracion del moodel nos descargamos una carpeta y la descomprimimos</p>
<p>Dentro de ellas esta lo siguientes archivos</p>
<img width="749" height="32" alt="image" src="https://github.com/user-attachments/assets/76fbac39-eeb4-43d6-a30f-2b11b645a936" class="course-image"/>
<p>Despues de esto ejecutamos un dpkg --reconfigure slapd para reconfigurar el servidor </p>
<img width="1028" height="609" alt="image" src="https://github.com/user-attachments/assets/a214d1f2-f598-4b84-a44d-5babb4a33abd" class="course-image"/>


    
<img width="1913" height="1045" alt="image" src="https://github.com/user-attachments/assets/79aef0d6-7e14-4af2-83d6-c08b6c761428" class="course-image"/>
<img width="1920" height="1039" alt="image" src="https://github.com/user-attachments/assets/68a8d204-742a-4cd4-a0d5-9c97b28e33b0" class="course-image"/>
<img width="1920" height="1002" alt="image" src="https://github.com/user-attachments/assets/b0a42273-3a74-4de1-af58-1cc5090e2ab0" class="course-image"/>
<img width="1011" height="159" alt="image" src="https://github.com/user-attachments/assets/dad9c00c-0beb-468f-81d7-93e1d134d876" class="course-image"/>
<img width="1920" height="654" alt="image" src="https://github.com/user-attachments/assets/78b6bb87-a20b-447e-846f-c6e85d7688ca" class="course-image"/>



</main>

<button id="scrollToTopBtn" class="scroll-btn" title="Ir arriba"><i class="fa-solid fa-arrow-up"></i></button>
<button id="scrollToBottomBtn" class="scroll-btn" title="Ir abajo"><i class="fa-solid fa-arrow-down"></i></button>

<script>
// Botón para subir
document.getElementById('scrollToTopBtn').addEventListener('click', function() {
    window.scrollTo({
        top: 0,
        behavior: 'smooth'
    });
});

// Botón para bajar
document.getElementById('scrollToBottomBtn').addEventListener('click', function() {
    window.scrollTo({
        top: document.body.scrollHeight,
        behavior: 'smooth'
    });
});

// Mostrar/ocultar botones según la posición del scroll
window.addEventListener('scroll', function() {
    const scrollTopBtn = document.getElementById('scrollToTopBtn');
    const scrollBottomBtn = document.getElementById('scrollToBottomBtn');
    
    // Mostrar botón de subir solo si no estamos arriba del todo
    if (window.pageYOffset > 300) {
        scrollTopBtn.style.opacity = '1';
        scrollTopBtn.style.pointerEvents = 'auto';
    } else {
        scrollTopBtn.style.opacity = '0.3';
    }
    
    // Mostrar botón de bajar solo si no estamos abajo del todo
    if ((window.innerHeight + window.pageYOffset) < document.body.scrollHeight - 100) {
        scrollBottomBtn.style.opacity = '1';
        scrollBottomBtn.style.pointerEvents = 'auto';
    } else {
        scrollBottomBtn.style.opacity = '0.3';
    }
});
</script>

<style>
:root {
    /* Ruta corregida con Liquid para Jekyll */
    --bg-image: url('{{ "/assetscss/pract2.gif" | relative_url }}');
}

body {
    background-image: var(--bg-image);
    background-size: cover;
    background-position: center center;
    background-attachment: fixed;
    overflow-x: hidden;
    overflow-y: auto;
}

code {
    background-color: #282c34;
    color: #abb2bf;
    padding: 2px 6px;
    border-radius: 4px;
    font-family: 'Courier New', Courier, monospace;
    font-size: 0.9em;
}

.navigation-links {
    position: sticky;
    top: 1rem;
    z-index: 100;
    background: var(--overlay);
    backdrop-filter: blur(8px);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    box-shadow: var(--shadow-neon);
    margin-top: 1.5rem;
    margin-bottom: 1.5rem;
    padding: 1rem 1.2rem;
    display: flex;
    justify-content: space-around;
    align-items: center;
    gap: 1rem;
}

.navigation-links a {
    text-decoration: none;
    color: var(--accent);
    transition: all 0.3s ease;
}

.navigation-links a:hover {
    color: var(--text);
    transform: translateY(-2px);
}

main.contenedor-principal {
    padding-bottom: 2.5rem;
}

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
</style>
