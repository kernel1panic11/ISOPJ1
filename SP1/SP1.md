# Unidad 1

Ubuntu como tal utiliza varios tipos de licencia, las que utiliza son las siguientes:

- 🔹 **Núcleo Linux** → Licencia **GPLv2** (GNU General Public License, versión 2).  
- 🔹 **Aplicaciones GNU** (coreutils, bash, etc.) → Principalmente **GPLv3** y otras licencias GNU (LGPL, AGPL, etc.).  
- 🔹 **Bibliotecas** → Muchas bajo **LGPL** (permite usarlas en software propietario).  
- 🔹 **Documentación** → Generalmente bajo **GNU Free Documentation License (GFDL)** o **Creative Commons**.  
- 🔹 **Paquetes incluidos** → Cada paquete conserva su propia licencia (puede ser GPL, MIT, Apache, BSD, etc.).  
- 🔹 **Marcas registradas de Ubuntu** → El software es libre, pero los logos, nombre y *branding* de Ubuntu están bajo las **Ubuntu Trademark Guidelines** de Canonical.  

---

# Instalación de Ubuntu

Primero iniciamos la ISO, en mi caso estoy usando **VMware** (me es más cómodo).

Empezamos creando la tabla de particiones.  
Por defecto viene **MSDOS** por tema de compatibilidad, pero lo he cambiado a **GPT** ya que puede soportar discos duros con mucho mayor volumen.

![Tabla GPT](https://github.com/user-attachments/assets/bc0a7fd2-dffc-40a3-a2ee-62d773f53012)

Aquí he creado la primera partición, que será la **home** y será de 20 GB.

![Partición home](https://github.com/user-attachments/assets/e8813fc1-d89c-4024-8009-0ec6ee1109f3)

Ahora creamos la **swap**, en mi caso le pondré 1 GB.

![Partición swap](https://github.com/user-attachments/assets/fda501cb-dac0-4033-a51c-30ad34dd693e)

Y ahora estamos creando la **raíz (/)**.

![Partición raíz](https://github.com/user-attachments/assets/979c2479-2bf9-4114-956b-3d85937224a1)

Resultado final:

![Particiones](https://github.com/user-attachments/assets/abf758cc-b67a-4593-aa73-c25040783cb0)

Aplicamos los cambios:

![Aplicar cambios](https://github.com/user-attachments/assets/277f1508-19aa-4df1-8c1c-3a15de066b88)

Marcamos la raíz:

![Raíz](https://github.com/user-attachments/assets/bd455cfe-09f9-4094-ac63-12d5a43dbc92)

Marcamos la **swap**:

![Swap](https://github.com/user-attachments/assets/0de7225b-03fd-4898-b889-2f37f34c75fd)

Marcamos la **home**:

![Home](https://github.com/user-attachments/assets/b8e48d67-d849-4339-98c3-a64c8d93f9ef)

Durante el proceso me di cuenta de que me había dejado la partición **boot**.  
He vuelto al editor, como se ve en la foto, y la he creado.  
Esta partición es necesaria para que el sistema operativo pueda arrancar.

![Boot](https://github.com/user-attachments/assets/8eee9a3d-adbf-486b-b05e-6eba551ab3a3)

Le indico qué partición es la **boot**:

![Asignar boot](https://github.com/user-attachments/assets/bd7d7a20-996e-4b85-8182-95f790a784b2)

Comienza la instalación:

![Instalación 1](https://github.com/user-attachments/assets/7757bdb8-230b-4c3b-8e49-87ce11263de0)  
![Instalación 2](https://github.com/user-attachments/assets/d54c95f1-cbcf-49c8-877a-156e1ee1a951)

---

En mi caso, como se puede observar, no estoy utilizando las opciones de clase.  
Se nota por la interfaz gráfica: he utilizado **Pop!_OS**, que está basado en **Ubuntu 22.05 LTS**, y el sistema de archivos es **Btrfs**.  

He elegido estas opciones por los siguientes motivos:

- **Sistema de archivos Btrfs** → permite crear *snapshots* del sistema sin tener que hacer copias de seguridad completas.  
  Si una actualización falla, puedo revertir los cambios de forma rápida y segura.  
- **Pop!_OS en lugar de Ubuntu estándar**:
  - Preferencia personal: me resulta más amigable visualmente.  
  - Razones técnicas: viene con más *drivers* que Ubuntu no trae por defecto y no usa *Snap*, sino **APT**, que rinde mucho mejor.  
  *Snap* funciona como una máquina virtual para cada programa, mientras que APT gestiona paquetes de forma más eficiente.  
