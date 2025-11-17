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
                    <li>Bajo nivel: Lo que hace es borrar todo, sobrescribe con 000000 todo e intenta arreglar los sectores defectuosos, pero se necesitan programas específicos, no se puede hacer a través del sistema operativo.</li>
                    <li>Medio nivel: Sería un intermedio, no borra archivos, lo que sí encuentra sectores defectuosos lo marca, pero no los intenta arreglar, también se pueden recuperar los archivos.</li>
                    <li>Alto nivel: no borra los archivos, solo borra el sistema de archivos y los sectores defectuosos que puede encontrar los ignora.</li>
                </ul>
            </li>
        </ul>
    </div>
    <div class="content-section">
        <h2 class="sub">Gestión de particiones:</h2>
        <p>Se puede hacer de dos formas usando:</p>
        <ul>
            <li>Gparted</li>
            <li>Comandos</li>
        </ul>
        <p>Aunque como he mencionado antes hay ciertas partes que se tienen que hacer por terminal ya que por GUI no se puede</p>
    </div>
    <div>
        <p>Para empezar, crearemos un disco virtual nuevo en vmware, lo normal es crear una sata pero en mi caso le he indicado NVME ya que así va más <strong>rápido</strong>, como se puede ver, el disco duro es de 10gb </p>
        <img width="826" height="670" alt="image" src="https://github.com/user-attachments/assets/d2fce307-de10-4ac3-80e6-8d1ea669cd9e" class="course-image"/>
        <p>Ahora comprobamos el sector del nuevo disco sin formatear con <code>fdisk -l</code>, como se puede ver el sector es de 512 bytes </p>
        <img width="933" height="569" alt="image" src="https://github.com/user-attachments/assets/ff7b4574-eb1e-48bc-a71a-11da2425f37f" class="course-image"/>
        <p>Ahora con el comando: <code>tune2fs -l (disco)</code> podemos ver el tamaño del bloque</p>
        <img width="621" height="128" alt="image" src="https://github.com/user-attachments/assets/a3a3b284-07db-4318-9f51-bd2eb2005c3d" class="course-image"/>
        <p>Ahora con <code>df -Th</code>, podemos ver el sistema de archivos que se usa, el espacio que <strong>está</strong> usado y el libre que hay, el poner h es opcional, pero lo pone "para humanos" entonces los representa en gb y así es <strong>más</strong> <strong>fácil</strong> de interpretar</p>
        <img width="699" height="180" alt="image" src="https://github.com/user-attachments/assets/ca0866b5-6e8f-4668-b300-59c24d8ac9a7" class="course-image"/>
        <p>La medida de clúster (Windows) y bloque (Linux), es la unidad mínima lógica donde se guardan los datos a nivel de sistema operativo, por defecto son 4096 bytes (es igual a 8 sectores) y esta medida <strong>sí</strong> que se puede cambiar, esta medida se puede cambiar cuando se formatea la partición, y cada partición del disco, <strong>puede</strong> tener una medida de bloque y sistema de archivos <strong>diferentes.</strong> Este es el claro ejemplo, podemos ver <strong>cómo</strong> el archivo pesa 17 y luego su tamaño real es de 5kb</p>
        <img width="682" height="191" alt="image" src="https://github.com/user-attachments/assets/04eca4eb-684d-45af-8bc4-ec36fbfd739c" class="course-image"/>
        <strong>Particiones</strong>: una <strong>partición</strong>, es un trozo <strong>físico</strong> del disco duro, pero no podemos modificar la medida del bloque, eso se hace por terminal, un volumen es una capa de <strong>abstracción</strong> que se pone encima de las particiones y/o discos. 
        El gparted sirve para gestionar <strong>particiones</strong>
        <img width="777" height="532" alt="image" src="https://github.com/user-attachments/assets/53f4f163-3724-4527-8ade-8cdf99407de5" class="course-image"/>
        <p>Aquí hemos creado dos particiones de <strong>más</strong> o menos 5 gb cada una, esto lo hemos hecho usando la terminal, como se puede ver en la foto, las dos particiones se han hecho</p>
        <img width="931" height="761" alt="image" src="https://github.com/user-attachments/assets/d17a2000-1cb1-48ef-9594-029f21bfeba1" class="course-image"/>
        <p>Aquí lo que he hecho es darle un formato a la primera <strong>partición</strong> en ext4, usando el comando <code>mkfs.ext4 -b 2048 /dev/nvme0n1p1</code></p>
        <img width="893" height="294" alt="image" src="https://github.com/user-attachments/assets/8f70f76c-b256-4adf-b9ed-ca11e9f471be" class="course-image"/>
        <p>Ahora aquí lo que he hecho es darle un formato a la <strong>partición</strong> secundaria en <strong>NTFS</strong>, usando el comando <code>mkfs.ntfs /dev/nvme0n1p1</code>, en este caso ha tardado <strong>más</strong> que con ext4 ya que tiene que llenar la <strong>partición</strong> de <strong>ceros</strong>, desconozco el <strong>porqué</strong></p>
        <img width="563" height="134" alt="image" src="https://github.com/user-attachments/assets/9ffc4cf6-337d-4a5a-b2e7-be66dabf4a9e" class="course-image"/>
        <p>Comprobando por terminal que se han hecho bien las particiones: </p>
        <img width="1039" height="201" alt="image" src="https://github.com/user-attachments/assets/e6fbdcd9-fe1a-4280-b15b-b4c410f46e85" class="course-image"/>
        <p>Y ahora lo comprobamos con gparted</p>
        <img width="771" height="257" alt="image" src="https://github.com/user-attachments/assets/10f5d46d-f9bc-47cc-bfe4-5b89e0a004dc" class="course-image"/>
    </div>
    <div>
        <h2 class="sub">Montaje de particiones:</h2>    
        <p>Montaje de <strong>partición</strong> en modo temporal</p>
        <p> Para empezar, montaremos de forma temporal una <strong>partición</strong> en <code>/mnt/p1</code>, <strong>podríamos</strong> montarla en cualquier otra carpeta, pero lo haremos aquí, para montar la <strong>partición</strong> en este caso, lo que <strong>haríamos</strong> es con el comando <code>mount -t ext4 /dev/nvme0n1p1 /mnt/p1</code> y dentro de la <strong>partición</strong> <strong>recién</strong> montada, lo que <strong>haré</strong> es crear una carpeta llamada "test" para luego reiniciar la vm y comprobar si sigue montada la <strong>partición</strong> y aparece la carpeta, lo cual ya adelanto, no es el caso </p>
        <p> Nota: Cuando utilizas el comando <code>mount</code>, incluir la opción <code>-t ext4</code> <strong>especifica</strong> explícitamente el sistema de archivos (en este caso, <code>ext4</code>) de la partición que deseas montar.
        Aunque <code>mount</code> sí puede intentar adivinar automáticamente el sistema de archivos, no siempre lo hace o no siempre lo hace correctamente, especialmente en sistemas que tienen múltiples tipos de sistemas de archivos.</p>
        <img width="624" height="144" alt="image" src="https://github.com/user-attachments/assets/56fe0548-4eb7-4d4f-a550-8653953e5818" class="course-image"/>
        <img width="338" height="31" alt="image" src="https://github.com/user-attachments/assets/8cabda37-5690-4f65-8d4a-57bae83564c5" class="course-image"/>
        Montaje de <strong>partición</strong> en modo permanente:
        <img width="808" height="256" alt="image" src="https://github.com/user-attachments/assets/93f4df08-d910-4f46-9f6b-04daf5424912" class="course-image"/>
        <strong>Comprobación</strong> <strong>después</strong> del reinicio
        <img width="820" height="179" alt="image" src="https://github.com/user-attachments/assets/732511da-07a3-4228-ac73-ee2b77ba7ca0" class="course-image"/>
        <strong>Desfragmentación</strong>
        <img width="1288" height="514" alt="image" src="https://github.com/user-attachments/assets/f9e8f8d2-03d9-4d99-8342-e5669b44f21f" class="course-image"/>
    </div>       
    <div class="content-section">
        <h2 class="sub">Gestión de procesos</h2>
    </div>
    <div class="content-section">
        <h2 class="sub">Gestión de usuarios, grupos y permisos</h2>
        <p>En el archivo /etc/passwd es donde están todos los usuarios del sistema </p>
        <img width="819" height="353" alt="image" src="https://github.com/user-attachments/assets/5805d903-ca58-47eb-88a6-647319ef0a19" class="course-image"/>
        <p>En /etc/group están todos los grupos y los usuarios que forman parte de este grupo</p>
        <img width="771" height="370" alt="image" src="https://github.com/user-attachments/assets/ab803637-a1d6-46e4-ab59-c69bf64950eb" class="course-image"/>
        <p>En /etc/shadow, se encuentran las contraseñas de los usuarios encriptadas, se puede manipular el algoritmo usado en el sistema editando otro archivo, también se ocupa de controlar la caducidad de las contraseñas</p>
        <img width="806" height="372" alt="image" src="https://github.com/user-attachments/assets/1bf21b36-436b-4512-bc61-b40ab12d5d49" class="course-image"/>
        <p>/etc/gshadow, aquí se gestionan las contraseñas del grupo y también se puede ver el usuario administrador de un grupo, solo puede haber un único admin por grupo, si se pone otro se tiene que quitar el anterior</p>
        <img width="834" height="431" alt="image" src="https://github.com/user-attachments/assets/1fefee5b-6444-4431-a69d-aa789a822610" class="course-image"/>
    <p>Aquí lo que hago es crear un usuario con adduser, con este comando automáticamente se te creará la carpeta home, pero no los archivos dentro de la home, eso se crearán cuando inicies sesión con ello</p>
    <img width="760" height="424" alt="image" src="https://github.com/user-attachments/assets/fb1c3fb9-91dc-4a40-81f4-ae7d782b9acc" class="course-image"/>
    <p> Cuando creas el usuario, no se crean las carpetas de la home hasta que inicies sesión con el usuario</p>
  nbsp; <img width="793" height="112" alt="image" src="https://github.com/user-attachments/assets/b921cc91-ab98-4dcf-bf7c-c6535eb8de12" class="course-image"/>
    <p>Ahora que hemos creado el usuario con useradd no crea la home y la shell es sh en vez de bash, aquí lo cambiamos</p>
<img width="386" height="80" alt="image" src="https://github.com/user-attachments/assets/7d9c5a34-2c62-4e5e-9a59-8996416c6250" class="course-image"/>

<p>Y ahora cambiamos la shell con el comando usermod</p>
<img width="353" height="39" alt="image" src="https://github.com/user-attachments/assets/2c84959e-dde9-4bee-95d0-8ac03ab50002" class="course-image"/>
<p>Ahora cambiamos el dueño de la carpeta </p>
<img width="399" height="31" alt="image" src="https://github.com/user-attachments/assets/f0e8a615-1156-4852-9587-cefd89ba2511" class="course-image"/>
<img width="461" height="158" alt="image" src="https://github.com/user-attachments/assets/7cf8760e-73ea-410a-824e-c49f5492cee2" class="course-image"/>
<p>Añadir usuario a un grupo</p>
<img width="294" height="19" alt="image" src="https://github.com/user-attachments/assets/640ea1eb-f91c-41f8-b6f3-e287a06eaa56" class="course-image"/>
<p>Quitar usuario de un grupo</p>
<img width="302" height="50" alt="image" src="https://github.com/user-attachments/assets/7a54ec05-8c12-43ba-80ce-02f8dff1eac3" class="course-image"/>

<p>Añadir gina a sudo </p>
<img width="285" height="31" alt="image" src="https://github.com/user-attachments/assets/92018145-02f8-49f6-b519-5ffd0459fb93" class="course-image"/>
<p>Al borrar usuario, no te borra la home </p>
<img width="253" height="54" alt="image" src="https://github.com/user-attachments/assets/51ff854f-80af-4f96-8915-31f55e4d0d6e" class="course-image"/>
<p>Ahora borrando la home </p>
<img width="544" height="59" alt="image" src="https://github.com/user-attachments/assets/d298827a-ec8c-4018-9c54-82a2bb76d31f" class="course-image"/>
<p>El comando para bloquear un usuario</p>
<img width="469" height="52" alt="image" src="https://github.com/user-attachments/assets/ab70587a-0818-42ce-ab64-991dfee08ec0" />


<p>Para desbloquear un usuario:</p>
<img width="483" height="23" alt="image" src="https://github.com/user-attachments/assets/6e7cfcf0-c0e4-457b-8a76-0cf21de4e62d" />
<p>Modificar el nombre del grupo y borrar el grupo</p>
<img width="376" height="111" alt="image" src="https://github.com/user-attachments/assets/3802a252-2b9b-472f-8192-792bcb57d0f7" class="course-image"/>
<p>Añadir usuarios a grupos de 3 formas diferentes</p>
<img width="390" height="126" alt="image" src="https://github.com/user-attachments/assets/d0f20f04-a006-4dcd-b71f-c9931d32fbba" class="course-image"/>
<p>Usuario admin con gshadow</p>
<img width="377" height="67" alt="image" src="https://github.com/user-attachments/assets/ef26d36f-2103-462b-bdea-f0b55ecaefa4" class="course-image"/>
<img width="477" height="147" alt="image" src="https://github.com/user-attachments/assets/ad3bbeff-a1ae-4fda-85f5-40a5394f556f" class="course-image"/>
<p>Con este se modifica el grupo principal del usuario</p>
<img width="381" height="64" alt="image" src="https://github.com/user-attachments/assets/23413d00-c98f-427a-8b29-2dc97565e397" class="course-image"/>
<p>Ahora hemos creado un grupo pero le hemos editado el nombre al correcto</p>
<img width="541" height="126" alt="image" src="https://github.com/user-attachments/assets/6d87fb40-1866-4aa1-8d02-fdd01f92df19" class="course-image"/>
<p> Añadiendo y eliminando usuarios de un grupo de tres formas difrentes</p>
<img width="634" height="379" alt="image" src="https://github.com/user-attachments/assets/be4f3977-0ed6-4df8-94ea-9cde25aeb053" class="course-image"/>
<p>Este comando sirve para establecer el grupo principal de un usuario, un usuario puede estar en un unico grupo principal pero en todos los que quiera secundarios</p>
<img width="551" height="141" alt="image" src="https://github.com/user-attachments/assets/cb66469f-eca2-47bd-8c39-3f8bd52fc635" class="course-image"/>
<p>Siempre puedes borrar grupos y a los usuarios no les pasa nada, excepto cuando este grupo sea el principal de un usuario</p>
<img width="732" height="112" alt="image" src="https://github.com/user-attachments/assets/18537ae3-a8be-4aa0-a8b1-29348496d50c" class="course-image"/>




        <ul>
            <li>&gt;&gt;Copias de seguridad y automatización de tareas</li>
            <li>&gt;&gt;Cuotas de usuario</li>
        </ul>
    </div>
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

// Mostrar/ocultar botones según la posición del scroll (opcional)
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
    /* Esto anula el fondo por defecto y pone el de pract2.gif */
    /* Ruta corregida con Liquid para Jekyll */
    --bg-image: url('{{ "/assetscss/pract2.gif" | relative_url }}');
}

/* --- AÑADIDO PARA FIJAR EL FONDO --- */
body {
    background-image: var(--bg-image);
    background-size: cover;
    background-position: center center;
    background-attachment: fixed;
    /* Esto ayuda a que 'position: sticky' funcione si el body tenía 'overflow: hidden' */
m   overflow-x: hidden;
    overflow-y: auto;
}

/* --- ESTILO AÑADIDO PARA LOS COMANDOS --- */
code {
    background-color: #282c34; /* Un fondo oscuro */
    color: #abb2bf; /* Un color de texto gris claro */
    padding: 2px 6px;
    border-radius: 4px;
    font-family: 'Courier New', Courier, monospace;
    font-size: 0.9em;
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
    display: flex;
    justify-content: space-around;
    align-items: center;
    gap: 1rem;￼
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

