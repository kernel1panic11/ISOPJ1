---
layout: default
title: "Unidad 1: Introducción a los Sistemas Operativos y Planificación"
---

<main class="contenedor-principal">
    
    <h1 class="titulo">Unidad 1. Introducción a los Sistemas Operativos y Planificación</h1>
    <div class="loading-bar"><div class="loading-progress"></div></div>

    <div class="content-section">
        <h2 class="sub">Tipos de Licencia en Ubuntu</h2>
        <p>
            Ubuntu como tal utiliza varios tipos de licencia, las que utiliza son las siguientes:
        </p>
        <ul>
            <li>Núcleo Linux → Licencia GPLv2 (GNU General Public License, versión 2).</li>
            <li>Aplicaciones GNU (coreutils, bash, etc.) → Principalmente GPLv3 y otras licencias GNU (LGPL, AGPL, etc.).</li>
            <li>Bibliotecas → Muchas bajo LGPL (permite usarlas en software propietario).</li>
            <li>Documentación → Generalmente bajo GNU Free Documentation License (GFDL) o Creative Commons.</li>
            <li>Paquetes incluidos → Cada paquete conserva su propia licencia (puede ser GPL, MIT, Apache, BSD, etc.).</li>
            <li>Marcas registradas de Ubuntu → El software es libre, pero los logos, nombre y branding de Ubuntu están bajo las Ubuntu Trademark Guidelines de Canonical.</li>
        </ul>
    </div>

    <div class="content-section">
        <h2 class="sub">Instalación de Ubuntu en VMware</h2>
        <p>
            Primero iniciamos la ISO, en mi caso estoy usando VMware (me es más cómodo).
        </p>
        <p>
            Empezamos creando la tabla de particiones. Por defecto viene MSDOS por tema de compatibilidad, pero lo he cambiado a GPT ya que puede soportar discos duros con mucho mayor volumen.
        </p>
        <img src="https://github.com/user-attachments/assets/bc0a7fd2-dffc-40a3-a2ee-62d773f53012" alt="Tabla GPT" class="course-image" />
        
        <p>Aquí he creado la primera partición, que será la home y será de 20 GB.</p>
        <img src="https://github.com/user-attachments/assets/e8813fc1-d89c-4024-8009-0ec6ee1109f3" alt="Partición home" class="course-image" />

        <p>Ahora creamos la swap, en mi caso le pondré 1 GB.</p>
        <img src="https://github.com/user-attachments/assets/fda501cb-dac0-4033-a51c-30ad34dd693e" alt="Partición swap" class="course-image" />

        <p>Y ahora estamos creando la raíz (/).</p>
        <img src="https://github.com/user-attachments/assets/979c2479-2bf9-4114-956b-3d85937224a1" alt="Partición raíz" class="course-image" />

        <h4>Resultado final de las particiones:</h4>
        <img src="https://github.com/user-attachments/assets/abf758cc-b67a-4593-aa73-c25040783cb0" alt="Particiones" class="course-image" />

        <p>Durante el proceso me di cuenta de que me había dejado la partición boot. He vuelto al editor y la he creado, ya que es necesaria para que el sistema operativo pueda arrancar.</p>
        <img src="https://github.com/user-attachments/assets/8eee9a3d-adbf-486b-b05e-6eba551ab3a3" alt="Creación de partición boot" class="course-image" />
    </div>

    <div class="content-section">
        <h2 class="sub">Post-Instalación y Personalización</h2>
        <p>
            Después de instalar, se puede ver que el hostname es "pop-os". Lo voy a cambiar editando el archivo <code>/etc/hostname</code>.
        </p>
        <img width="1283" height="918" alt="Editando el archivo de hostname" src="https://github.com/user-attachments/assets/47756089-5f51-4f5f-a99a-d271bfc01505" class="course-image" />
        <p>
            Una vez modificado, reiniciamos la máquina virtual para aplicar los cambios. El resultado es el nuevo hostname:
        </p>
        <img width="1270" height="833" alt="Resultado del nuevo hostname en la terminal" src="https://github.com/user-attachments/assets/4563312d-1c12-455e-9f85-e4fae4da302d" class="course-image" />
    </div>

    <div class="content-section">
        <h2 class="sub">Elección de Pop!_OS y Btrfs</h2>
        <p>
            Como se puede observar, no estoy utilizando una instalación estándar de Ubuntu. He elegido Pop!_OS (basado en Ubuntu 22.04 LTS) con el sistema de archivos Btrfs por los siguientes motivos:
        </p>
        <ul>
            <li>
                Sistema de archivos Btrfs: Permite crear snapshots del sistema sin tener que hacer copias de seguridad completas. Si una actualización falla, puedo revertir los cambios de forma rápida y segura.
            </li>
            <li>
                Pop!_OS en lugar de Ubuntu estándar:
                <ul>
                    <li>Preferencia personal: Me resulta más amigable visualmente.</li>
                    <li>Razones técnicas: Viene con más drivers preinstalados y no usa Snap, sino APT, que en mi opinión rinde mucho mejor.</li>
                </ul>
            </li>
        </ul>
    </div>

    <div class="content-section"> 
        <h2 class="sub">Restaurando el GRUB</h2>
        <p>Ahora lo que hago es instalar Windows 11 para que sobrescriba el GRUB de Ubuntu, para luego poderlo restaurar más tarde.</p>
        <p>Ahora empezamos la instalación de Windows 11:</p>
        <img width="965" height="675" alt="Comienzo de la instalación de Windows 11" src="https://github.com/user-attachments/assets/0d92dbdc-aa5a-4ce4-94bf-8374aa783d78" class="course-image" />
        <p>Seleccionamos el disco duro:</p>
        <img width="887" height="663" alt="Selección del disco duro para instalar Windows" src="https://github.com/user-attachments/assets/57788b14-88d2-4920-ab7f-772f8d86f83d" class="course-image" />
        <p>Empezamos la instalación:</p>
        <img width="1270" height="796" alt="Proceso de instalación de Windows" src="https://github.com/user-attachments/assets/4d5c311c-0498-482b-98ac-2a955f082184" class="course-image" />
        <p>Ya ha terminado la instalacion de windows 11 </p> 
        <img width="1712" height="880" alt="image" src="https://github.com/user-attachments/assets/2c9c7395-cc71-4eb8-8a79-d217f4c81139" />
        <p>Aquí dejo un video como se puede ver que el unico sistema que arranca es Windows 11</p>
        <video src="2025-10-02 12-30-44.mp4" controls width="100%"></video>
    </div>


</main>
