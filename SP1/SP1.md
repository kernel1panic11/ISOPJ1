layout: default
title: "Unidad 1: Virtualización e Instalación"
---

<main class="contenedor-principal">
    <h1 class="titulo">Unidad 1</h1>
Ubuntu como tal utiliza varios tipos de licencia, las que utiliza son las siguientes: 

🔹 Núcleo Linux → Licencia GPLv2 (GNU General Public License, versión 2).

🔹 Aplicaciones GNU (coreutils, bash, etc.) → Principalmente GPLv3 y otras licencias GNU (LGPL, AGPL, etc.).

🔹 Bibliotecas → Muchas bajo LGPL (permite usarlas en software propietario).

🔹 Documentación → Generalmente bajo GNU Free Documentation License (GFDL) o Creative Commons.

🔹 Paquetes incluidos → Cada paquete conserva su propia licencia (puede ser GPL, MIT, Apache, BSD, etc.).

🔹 Marcas registradas de Ubuntu → El software es libre, pero los logos, nombre y branding de Ubuntu están bajo las Ubuntu Trademark Guidelines de Canonical.

 <h1 class="titulo">Instalacion de Ubuntu</h1>

Primero inciamos la iso, en mi caso estoy usando vmware (me es mas comodo)

Empezamos creando la tabla de peraticiones, por defecto viene MSDOS por tema de compatiblidad, pero en lo he cambiado a gpt ya que puede soportar discos duros con mucho mayor volumen

<img width="1474" height="920" alt="image" src="https://github.com/user-attachments/assets/bc0a7fd2-dffc-40a3-a2ee-62d773f53012" />

Aquí he creado la primera particion, que sera la home y sera de 20 gb 

<img width="1281" height="864" alt="image" src="https://github.com/user-attachments/assets/e8813fc1-d89c-4024-8009-0ec6ee1109f3" />

Ahora creamos la swap, en mi caso le pondre un gb de swap 

<img width="1476" height="913" alt="image" src="https://github.com/user-attachments/assets/fda501cb-dac0-4033-a51c-30ad34dd693e" />

Y ahora estamos creando la raiz (/)

<img width="1475" height="917" alt="image" src="https://github.com/user-attachments/assets/979c2479-2bf9-4114-956b-3d85937224a1" />

Y el resultado: 

<img width="1481" height="897" alt="image" src="https://github.com/user-attachments/assets/abf758cc-b67a-4593-aa73-c25040783cb0" />

Ahora aplicamos los cambios 

<img width="882" height="628" alt="image" src="https://github.com/user-attachments/assets/277f1508-19aa-4df1-8c1c-3a15de066b88" />


En mi caso, como se puede observar, no estoy utilizando las opciones convencionales, se nota por la interfaz grafica, he utilizado popOS! que esta basado en ubuntu 22.05 LTS, y el sistema de archivos es BTRFS, he elegido estas opciones por lo siguientes motivos 


</main>
