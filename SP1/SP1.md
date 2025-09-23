<main class="contenedor-principal">
    <h1 class="titulo">Unidad 1</h1>
    <p>Ubuntu como tal utiliza varios tipos de licencia, las que utiliza son las siguientes:</p>
    <ul>
        <li>🔹 Núcleo Linux → Licencia GPLv2 (GNU General Public License, versión 2).</li>
        <li>🔹 Aplicaciones GNU (coreutils, bash, etc.) → Principalmente GPLv3 y otras licencias GNU (LGPL, AGPL, etc.).</li>
        <li>🔹 Bibliotecas → Muchas bajo LGPL (permite usarlas en software propietario).</li>
        <li>🔹 Documentación → Generalmente bajo GNU Free Documentation License (GFDL) o Creative Commons.</li>
        <li>🔹 Paquetes incluidos → Cada paquete conserva su propia licencia (puede ser GPL, MIT, Apache, BSD, etc.).</li>
        <li>🔹 Marcas registradas de Ubuntu → El software es libre, pero los logos, nombre y <i>branding</i> de Ubuntu están bajo las Ubuntu Trademark Guidelines de Canonical.</li>
    </ul>

    <h1 class="titulo">Instalación de Ubuntu</h1>
    <p>Primero iniciamos la ISO, en mi caso estoy usando VMware (me es más cómodo).</p>
    <p>Empezamos creando la tabla de particiones, por defecto viene MSDOS por tema de compatibilidad, pero lo he cambiado a GPT ya que puede soportar discos duros con mucho mayor volumen.</p>

    <img width="1474" height="920" alt="image" src="https://github.com/user-attachments/assets/bc0a7fd2-dffc-40a3-a2ee-62d773f53012" />

    <p>Aquí he creado la primera partición, que será la <i>home</i> y será de 20 GB.</p>

    <img width="1281" height="864" alt="image" src="https://github.com/user-attachments/assets/e8813fc1-d89c-4024-8009-0ec6ee1109f3" />

    <p>Ahora creamos la <i>swap</i>, en mi caso le pondré un GB de <i>swap</i>.</p>

    <img width="1476" height="913" alt="image" src="https://github.com/user-attachments/assets/fda501cb-dac0-4033-a51c-30ad34dd693e" />

    <p>Y ahora estamos creando la raíz (/).</p>

    <img width="1475" height="917" alt="image" src="https://github.com/user-attachments/assets/979c2479-2bf9-4114-956b-3d85937224a1" />

    <p>Y el resultado:</p>

    <img width="1481" height="897" alt="image" src="https://github.com/user-attachments/assets/abf758cc-b67a-4593-aa73-c25040783cb0" />

    <p>Ahora aplicamos los cambios.</p>

    <img width="882" height="628" alt="image" src="https://github.com/user-attachments/assets/277f1508-19aa-4df1-8c1c-3a15de066b88" />

    <p>Y ahora le marco donde quiero que me instale la raíz.</p>

    <img width="1336" height="799" alt="image" src="https://github.com/user-attachments/assets/bd455cfe-09f9-4094-ac63-12d5a43dbc92" />

    <p>La <i>swap</i>.</p>

    <img width="1106" height="774" alt="image" src="https://github.com/user-attachments/assets/0de7225b-03fd-4898-b889-2f37f34c75fd" />

    <p>La <i>home</i>.</p>

    <img width="1313" height="736" alt="image" src="https://github.com/user-attachments/assets/b8e48d67-d849-4339-98c3-a64c8d93f9ef" />

    <p>Y durante el proceso me he dado cuenta que me he dejado la partición "boot". He vuelto al editor como se ve en la foto y la he creado. Esta partición es para que el sistema operativo, como indica su nombre, pueda arrancar.</p>

    <img width="1152" height="739" alt="image" src="https://github.com/user-attachments/assets/8eee9a3d-adbf-486b-b05e-6eba551ab3a3" />

    <p>Y ahora le indico qué partición es la <i>boot</i>.</p>

    <img width="898" height="628" alt="image" src="https://github.com/user-attachments/assets/bd7d7a20-996e-4b85-8182-95f790a784b2" />

    <p>Y comenzamos con la instalación.</p>

    <img width="1318" height="860" alt="image" src="https://github.com/user-attachments/assets/7757bdb8-230b-4c3b-8e49-87ce11263de0" />
    <img width="1291" height="803" alt="image" src="https://github.com/user-attachments/assets/d54c95f1-cbcf-49c8-877a-156e1ee1a951" />

    <p>En mi caso, como se puede observar, no estoy utilizando las opciones convencionales, se nota por la interfaz gráfica, he utilizado Pop!_OS, que está basado en Ubuntu 22.05 LTS, y el sistema de archivos es Btrfs. He elegido estas opciones por los siguientes motivos: el sistema de archivos Btrfs es porque este sistema te permite crear <i>snapshots</i> del sistema sin tener que hacer copias de seguridad enteras. Entonces, si va mal una actualización, lo que hace la <i>snapshot</i> es hacer copia de seguridad de las versiones anteriores de los paquetes que se van a actualizar, y si sale mal puedo revertir los cambios, entonces es por seguridad. Y el sistema operativo es por preferencias personales y técnicas: las personales es porque me parece más amigable a la vista, y las técnicas es porque viene con muchos <i>drivers</i> que Ubuntu no tiene por defecto y tampoco viene con <i>Snap</i> por defecto. Utiliza APT, y APT rinde mucho más que <i>Snap</i>, ya que <i>Snap</i> es un programa en una máquina virtual.</p>
</main>
