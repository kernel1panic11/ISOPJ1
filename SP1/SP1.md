---
layout: default
title: "Unidad 1. Introducción a los Sistemas Operativos y Planificación"
---

<main class="contenedor-principal">
    <h1 class="titulo">Unidad 1. Introducción a los Sistemas Operativos y Planificación</h1>
    <div class="loading-bar"><div class="loading-progress"></div>
    <div class="content-section">
        <h2 class="sub">Tipos de Licencia en Ubuntu</h2>
        <p>
            Ubuntu como tal utiliza varios tipos de licencia, que son las siguientes:
        </p>
        <ul>
            <li><b>Núcleo Linux</b> → Licencia GPLv2 (GNU General Public License, versión 2).</li>
            <li><b>Aplicaciones GNU</b> (coreutils, bash, etc.) → Principalmente GPLv3 y otras licencias GNU (LGPL, AGPL, etc.).</li>
            <li><b>Bibliotecas</b> → Muchas bajo LGPL (permite usarlas en software propietario).</li>
            <li><b>Documentación</b> → Generalmente bajo GNU Free Documentation License (GFDL) o Creative Commons.</li>
            <li><b>Paquetes incluidos</b> → Cada paquete conserva su propia licencia (puede ser GPL, MIT, Apache, BSD, etc.).</li>
            <li><b>Marcas registradas de Ubuntu</b> → El software es libre, pero los logos, el nombre y el <i>branding</i> de Ubuntu están bajo las <i>Ubuntu Trademark Guidelines</i> de Canonical.</li>
        </ul>
    </div>
    <div class="content-section">
        <h2 class="sub">Instalación de Ubuntu en VMware</h2>
        <p>
            Primero, iniciamos la ISO. En mi caso, estoy usando VMware porque me resulta más cómodo.
        </p>
        <p>
            Empezamos creando la tabla de particiones. Por defecto, viene en formato <b>MSDOS</b> por temas de compatibilidad, pero la he cambiado a <b>GPT</b>, ya que puede soportar discos duros con un volumen mucho mayor.
        </p>
        <img src="https://github.com/user-attachments/assets/bc0a7fd2-dffc-40a3-a2ee-62d773f53012" alt="Tabla GPT" class="course-image" />
        
        <p>Aquí he creado la primera partición, que será la <code>/home</code> y tendrá 20 GB.</p>
        <img src="https://github.com/user-attachments/assets/e8813fc1-d89c-4024-8009-0ec6ee1109f3" alt="Partición home" class="course-image" />
        <p>Ahora creamos la partición <b>swap</b>, que en mi caso será de 1 GB.</p>
        <img src="https://github.com/user-attachments/assets/fda501cb-dac0-4033-a51c-30ad34dd693e" alt="Partición swap" class="course-image" />
        <p>Y ahora estamos creando la <b>raíz</b> (<code>/</code>).</p>
        <img src="https://github.com/user-attachments/assets/979c2479-2bf9-4114-956b-3d85937224a1" alt="Partición raíz" class="course-image" />
        <h4>Resultado final de las particiones:</h4>
        <img src="https://github.com/user-attachments/assets/abf758cc-b67a-4593-aa73-c25040783cb0" alt="Particiones" class="course-image" />
        <p>Durante el proceso, me di cuenta de que me había olvidado de la partición <code>/boot</code>. He vuelto al editor y la he creado, ya que es necesaria para que el sistema operativo pueda arrancar.</p>
        <img src="https://github.com/user-attachments/assets/8eee9a3d-adbf-486b-b05e-6eba551ab3a3" alt="Creación de partición boot" class="course-image" />
    </div>
    <div class="content-section">
        <h2 class="sub">Post-Instalación y Personalización</h2>
        <p>
            Después de instalar, se puede ver que el <i>hostname</i> es «pop-os». Lo voy a cambiar editando el archivo <code>/etc/hostname</code>.
        </p>
        <img width="1283" height="918" alt="Editando el archivo de hostname" src="https://github.com/user-attachments/assets/47756089-5f51-4f5f-a99a-d271bfc01505" class="course-image" />
        <p>
            Una vez modificado, reiniciamos la máquina virtual para aplicar los cambios. El resultado es el nuevo <i>hostname</i>:
        </p>
        <img width="1270" height="833" alt="Resultado del nuevo hostname en la terminal" src="https://github.com/user-attachments/assets/4563312d-1c12-455e-9f85-e4fae4da302d" class="course-image" />
    </div>
    <div class="content-section">
        <h2 class="sub">Elección de Pop!_OS y Btrfs</h2>
        <p>
            Como se puede observar, no estoy utilizando una instalación estándar de Ubuntu. He elegido <b>Pop!_OS</b> (basado en Ubuntu 22.04 LTS) con el sistema de archivos <b>Btrfs</b> por los siguientes motivos:
        </p>
        <ul>
            <li>
                <b>Sistema de archivos Btrfs</b>: Permite crear <i>snapshots</i> del sistema sin tener que hacer copias de seguridad completas. Si una actualización falla, puedo revertir los cambios de forma rápida y segura.
            </li>
            <li>
                <b>Pop!_OS en lugar de Ubuntu estándar</b>:
                <ul>
                    <li><b>Preferencia personal</b>: Me resulta más amigable visualmente.</li>
                    <li><b>Razones técnicas</b>: Viene con más <i>drivers</i> preinstalados y no usa Snap, sino APT, que en mi opinión rinde mucho mejor.</li>
                </ul>
            </li>
        </ul>
    </div>
    <div class="content-section"> 
        <h2 class="sub">Restaurando el GRUB</h2>
        <p>Ahora instalo Windows 11 para que sobrescriba el GRUB de Ubuntu y así poder restaurarlo más tarde.</p>
        <p>Empezamos la instalación de Windows 11:</p>
        <img width="965" height="675" alt="Comienzo de la instalación de Windows 11" src="https://github.com/user-attachments/assets/0d92dbdc-aa5a-4ce4-94bf-8374aa783d78" class="course-image" />
        <p>Seleccionamos el disco duro:</p>
        <img width="887" height="663" alt="Selección del disco duro para instalar Windows" src="https://github.com/user-attachments/assets/57788b14-88d2-4920-ab7f-772f8d86f83d" class="course-image" />
        <p>Comienza la instalación:</p>
        <img width="1270" height="796" alt="Proceso de instalación de Windows" src="https://github.com/user-attachments/assets/4d5c311c-0498-482b-98ac-2a955f082184" class="course-image" />
        <p>Aquí dejo un vídeo donde se puede ver que el único sistema que arranca es Windows 11:</p>
        <video src="2025-10-02 12-30-44.mp4" controls width="100%" class="course-image"></video>
        <p>Ahora, me dirijo a la UEFI de la máquina virtual y arranco desde el CD donde tengo <b>Super Grub2 Disk</b> para restaurar el GRUB.</p>
        <img width="708" height="501" alt="Acceso a la UEFI de la máquina virtual" src="https://github.com/user-attachments/assets/f9b144dc-789e-4629-9fe3-fec4ed53611b" class="course-image" />
        <p>En el menú de Super Grub2 Disk, elijo la opción «Detect and show boot methods».</p> 
        <img width="723" height="415" alt="Menú de Super Grub2 Disk" src="https://github.com/user-attachments/assets/b6a805aa-163c-4fdc-b9df-9349f2adae81" class="course-image" />    
        <p>Una vez aquí, hay que seleccionar la entrada de Linux para que arranque el sistema.</p>
        <img width="704" height="459" alt="Selección del sistema Linux" src="https://github.com/user-attachments/assets/06ddc8d9-ea7a-4789-bb7b-d92b53b36018" class="course-image" />
        <p>Una vez he iniciado Linux, voy a la terminal para buscar la partición de arranque.</p>
        <p>En mi caso, como se puede ver, la partición EFI donde se instalará el GRUB es <code>nvme0n1p1</code>.</p> 
        <img width="669" height="245" alt="image" src="https://github.com/user-attachments/assets/4756322d-e097-487d-a6a1-1bde5f1d21f8" />
        <p>Una vez localizada, instalo el GRUB en el disco principal (el gestor de arranque se instalará en la partición EFI automáticamente):</p>
        <pre><code>sudo grub-install /dev/nvme0n1</code></pre>
        <p>Y ahora ejecuto el siguiente comando para que el GRUB se actualice y detecte todos los sistemas operativos:</p>
        <pre><code>sudo update-grub</code></pre>
        <p>Como se puede comprobar, ahora en el menú del GRUB aparecen tanto Pop!_OS (identificado como Ubuntu) como Windows.</p>
        <img width="1143" height="748" alt="image" src="https://github.com/user-attachments/assets/3165cf72-ac23-4cd6-a3cf-bd603e370bd4" />
        <p>Para finalizar, me he tomado la libertad de instalar un tema para el GRUB.</p>
        <img width="1030" height="763" alt="image" src="https://github.com/user-attachments/assets/e6d2f939-98a7-47a0-b877-a6b107415d85" />
    </div>
</main>
