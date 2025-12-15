---
layout: default
title: "Sprint 2. Instalación, configuración de software base y gestión de ficheros."
---

<main class="contenedor-principal">
    <h1 class="titulo">Sprint 2. Instalación, configuración de software base y gestión de ficheros.</h1>
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
                    <li><strong>Bajo nivel</strong>: lo que hace es borrar todo; sobrescribe todo con 000000 e intenta reparar los sectores defectuosos, pero se necesitan programas específicos; no se puede hacer a través del sistema operativo de forma normal.</li>
                    <li><strong>Medio nivel</strong>: sería un intermedio, no borra archivos; los sectores defectuosos que encuentra los marca, pero no intenta arreglarlos. También se pueden recuperar los archivos.</li>
                    <li><strong>Alto nivel</strong>: no borra los archivos, solo borra el sistema de archivos e ignora los sectores defectuosos que pueda encontrar.</li>
                </ul>
            </li>
        </ul>
        <p>En la práctica, hoy en día se suele hablar más de <strong>formateo rápido</strong> (solo recrea las estructuras del sistema de archivos) y <strong>formateo completo</strong> (recorre el disco y marca sectores defectuosos). El “bajo nivel” real lo hace el fabricante del disco; las herramientas que usamos simulan este comportamiento escribiendo en toda la superficie.</p>
        <p>El tamaño del <strong>sector</strong> lo fija el fabricante del disco (por ejemplo, 512 B o 4 KB físicos), mientras que el tamaño del <strong>bloque</strong> o clúster se define al crear el sistema de archivos. Un tamaño de bloque más grande reduce tablas de asignación, pero desperdicia más espacio (más fragmentación interna). Un tamaño de bloque más pequeño aprovecha mejor el espacio, pero aumenta tablas y accesos.</p>
        <p>La <strong>fragmentación interna</strong> es el espacio que se pierde dentro de un bloque cuando el archivo no lo llena entero (por ejemplo, archivo de 17 B en un bloque de 4096 B). La <strong>fragmentación externa</strong> es cuando un archivo está repartido en muchos trozos no contiguos en el disco, lo que empeora el rendimiento al leerlo.</p>
    </div>
    <div class="content-section">
        <h2 class="sub">Gestión de particiones:</h2>
        <p>Se puede hacer de dos formas usando:</p>
        <ul>
            <li><strong>GParted</strong></li>
            <li><strong>Comandos</strong> (por ejemplo, <code>fdisk</code>, <code>parted</code> o <code>lsblk</code>)</li>
        </ul>
        <p>Aunque, como he mencionado antes, hay ciertas partes que se tienen que hacer por terminal, ya que por GUI no se puede (por ejemplo, algunas opciones de <code>mkfs</code>, trabajar por SSH, scripts, automatización, etc.).</p>
        <p>Además, es importante entender que el disco puede estar particionado con <strong>MBR</strong> (tablas de particiones antiguas, máximo 4 particiones primarias) o con <strong>GPT</strong> (más moderno, soporta muchas más particiones y discos de mayor tamaño). Herramientas como <code>gdisk</code> o <code>parted</code> detectan este esquema.</p>
    </div>
    <div>
        <p>Para empezar, crearemos un disco virtual nuevo en VMware. Lo normal es crear un disco SATA, pero en mi caso he indicado NVMe ya que así va más <strong>rápido</strong> (menos latencia y mejor rendimiento simulado). Como se puede ver, el disco duro es de 10 GB.</p>
        <img width="826" height="670" alt="image" src="https://github.com/user-attachments/assets/d2fce307-de10-4ac3-80e6-8d1ea669cd9e" class="course-image"/>
        <p>Ahora comprobamos el tamaño del sector del nuevo disco sin formatear con <code>fdisk -l</code>. Como se puede ver, el sector es de 512 bytes.</p>
        <img width="933" height="569" alt="image" src="https://github.com/user-attachments/assets/ff7b4574-eb1e-48bc-a71a-11da2425f37f" class="course-image"/>
        <p>Ahora, con el comando <code>tune2fs -l (disco)</code>, podemos ver el tamaño del bloque de una partición ya formateada con <code>ext4</code>.</p>
        <img width="621" height="128" alt="image" src="https://github.com/user-attachments/assets/a3a3b284-07db-4318-9f51-bd2eb2005c3d" class="course-image"/>
        <p>Ahora, con <code>df -Th</code>, podemos ver el sistema de archivos que se usa, el espacio que está usado y el libre que hay. El parámetro <code>-h</code> es opcional, pero lo pone “para humanos”; entonces lo representa en GB/MB y así es más fácil de interpretar.</p>
        <img width="699" height="180" alt="image" src="https://github.com/user-attachments/assets/ca0866b5-6e8f-4668-b300-59c24d8ac9a7" class="course-image"/>
        <p>La medida de clúster (Windows) y de bloque (Linux) es la unidad mínima lógica donde se guardan los datos a nivel de sistema operativo. Por defecto son 4096 bytes (es igual a 8 sectores) y esta medida sí se puede cambiar. Esta medida se cambia cuando se formatea la partición, y cada partición del disco puede tener una medida de bloque y un sistema de archivos diferentes.</p>
        <p>Este es un claro ejemplo: podemos ver cómo el archivo pesa 17 bytes y luego su tamaño real es de 5 KB; esa diferencia es la <strong>fragmentación interna</strong> que se pierde dentro del bloque.</p>
        <img width="682" height="191" alt="image" src="https://github.com/user-attachments/assets/04eca4eb-684d-45af-8bc4-ec36fbfd739c" class="course-image"/>
        <p><strong>Particiones</strong>: una <strong>partición</strong> es un trozo físico (contiguo) del disco duro, definido en la tabla de particiones. No podemos modificar la medida del bloque directamente; eso se hace al formatear con el sistema de archivos, normalmente por terminal. Un <strong>volumen</strong> es una capa de abstracción que se pone encima de las particiones y/o discos (por ejemplo, LVM, RAID, etc.).</p>
        <p>GParted sirve para gestionar particiones (crear, borrar, redimensionar, cambiar el tipo de sistema de archivos, ver flags como <code>boot</code>, etc.).</p>
        <img width="777" height="532" alt="image" src="https://github.com/user-attachments/assets/53f4f163-3724-4527-8ade-8cdf99407de5" class="course-image"/>
        <p>Aquí hemos creado dos particiones de más o menos 5 GB cada una. Esto lo hemos hecho usando la terminal. Como se puede ver en la imagen, las dos particiones se han creado correctamente.</p>
        <img width="931" height="761" alt="image" src="https://github.com/user-attachments/assets/d17a2000-1cb1-48ef-9594-029f21bfeba1" class="course-image"/>
        <p>Aquí lo que he hecho es darle formato a la primera partición en <strong>ext4</strong>, usando el comando <code>mkfs.ext4 -b 2048 /dev/nvme0n1p1</code>. El parámetro <code>-b 2048</code> indica un tamaño de bloque de 2048 bytes en lugar de los 4096 típicos.</p>
        <img width="893" height="294" alt="image" src="https://github.com/user-attachments/assets/8f70f76c-b256-4adf-b9ed-ca11e9f471be" class="course-image"/>
        <p>Ahora, aquí lo que he hecho es darle formato a la partición secundaria en <strong>NTFS</strong>, usando el comando <code>mkfs.ntfs /dev/nvme0n1p1</code>. En este caso ha tardado más que con ext4 ya que tiene que llenar la partición de ceros. Desconozco el por qué.</p>
        <p><strong>Nota</strong>: en realidad, al ser la segunda partición, el dispositivo correcto normalmente sería <code>/dev/nvme0n1p2</code>. Es importante fijarse bien al formatear, porque si te equivocas puedes borrar datos de otra partición.</p>
        <img width="563" height="134" alt="image" src="https://github.com/user-attachments/assets/9ffc4cf6-337d-4a5a-b2e7-be66dabf4a9e" class="course-image"/>
        <p>Comprobando por terminal que se han creado bien las particiones:</p>
        <img width="1039" height="201" alt="image" src="https://github.com/user-attachments/assets/e6fbdcd9-fe1a-4280-b15b-b4c410f46e85" class="course-image"/>
        <p>Y ahora lo comprobamos con GParted:</p>
        <img width="771" height="257" alt="image" src="https://github.com/user-attachments/assets/10f5d46d-f9bc-47cc-bfe4-5b89e0a004dc" class="course-image"/>
    </div>
    <div>
        <h2 class="sub">Montaje de particiones:</h2>
        <p><strong>Montaje de partición en modo temporal</strong></p>
        <p>Para empezar, montaremos de forma temporal una partición en <code>/mnt/p1</code>. Podríamos montarla en cualquier otra carpeta, pero lo haremos aquí. Para montar la partición en este caso, utilizaremos el comando <code>mount -t ext4 /dev/nvme0n1p1 /mnt/p1</code> y, dentro de la partición recién montada, crearé una carpeta llamada “test” para luego reiniciar la VM y comprobar si sigue montada la partición y aparece la carpeta, lo cual ya adelanto que no es el caso.</p>
        <p>Esto es porque, por defecto, los montajes hechos solo con <code>mount</code> son <strong>temporales</strong> y se pierden al reiniciar si no se configuran en <code>/etc/fstab</code>.</p>
        <p>Nota: cuando utilizas el comando <code>mount</code>, incluir la opción <code>-t ext4</code> especifica explícitamente el sistema de archivos (en este caso, <code>ext4</code>) de la partición que deseas montar.
        Aunque <code>mount</code> puede intentar adivinar automáticamente el sistema de archivos, no siempre lo hace o no siempre lo hace correctamente, especialmente en sistemas que tienen múltiples tipos de sistemas de archivos.</p>
        <img width="624" height="144" alt="image" src="https://github.com/user-attachments/assets/56fe0548-4eb7-4d4f-a550-8653953e5818" class="course-image"/>
        <img width="338" height="31" alt="image" src="https://github.com/user-attachments/assets/8cabda37-5690-4f65-8d4a-57bae83564c5" class="course-image"/>
        <p><strong>Montaje de partición en modo permanente:</strong></p>
        <p>Para que el montaje sea permanente, hay que editar el archivo <code>/etc/fstab</code> y añadir una línea con el dispositivo, el punto de montaje, el sistema de archivos y las opciones de montaje. De esta forma, cada vez que se inicie el sistema, la partición se montará automáticamente.</p>
        <img width="808" height="256" alt="image" src="https://github.com/user-attachments/assets/93f4df08-d910-4f46-9f6b-04daf5424912" class="course-image"/>
        <p><strong>Comprobación después del reinicio:</strong></p>
        <p>Tras reiniciar la máquina, comprobamos que el sistema ha montado la partición automáticamente y que la carpeta “test” sigue existiendo, lo que demuestra que el montaje permanente está bien configurado.</p>
        <img width="820" height="179" alt="image" src="https://github.com/user-attachments/assets/732511da-07a3-4228-ac73-ee2b77ba7ca0" class="course-image"/>
        <p><strong>Desfragmentación</strong></p>
        <p>En sistemas de archivos como <strong>NTFS</strong> (Windows) es habitual necesitar desfragmentación para mejorar el rendimiento cuando hay muchos archivos fragmentados. En sistemas como <strong>ext4</strong> la fragmentación suele ser menor gracias a cómo se diseñó el sistema de archivos, pero también existen herramientas (por ejemplo, <code>e4defrag</code>) para analizar y desfragmentar si es necesario.</p>
        <img width="1288" height="514" alt="image" src="https://github.com/user-attachments/assets/f9e8f8d2-03d9-4d99-8342-e5669b44f21f" class="course-image"/>
    </div>
    <div class="content-section">
        <h2 class="sub">Gestión de procesos</h2>
        <p>En Linux, los procesos se pueden gestionar con varios comandos:</p>
        <ul>
            <li><code>ps aux</code>: muestra la lista de procesos en ejecución con detalles (PID, usuario, CPU, memoria, etc.).</li>
            <li><code>top</code> o <code>htop</code>: muestran procesos en tiempo real y permiten ordenarlos por uso de CPU, memoria, etc.</li>
            <li><code>kill PID</code>: envía una señal a un proceso (por defecto <code>SIGTERM</code>) para que se cierre.</li>
            <li><code>kill -9 PID</code>: envía <code>SIGKILL</code> (forzada) cuando el proceso no responde a <code>SIGTERM</code>.</li>
            <li><code>nice</code> y <code>renice</code>: permiten ajustar la prioridad de los procesos.</li>
        </ul>
        <p>También es útil usar <code>systemctl</code> para gestionar servicios (que son procesos gestionados por <code>systemd</code>): <code>systemctl start</code>, <code>stop</code>, <code>restart</code>, <code>status</code>, etc.</p>
    </div>
    <div class="content-section">
        <h2 class="sub">Gestión de usuarios, grupos y permisos</h2>
        <p>En el archivo <code>/etc/passwd</code> es donde se encuentran todos los usuarios del sistema.</p>
        <p>Cada línea de <code>/etc/passwd</code> tiene campos separados por <code>:</code>, por ejemplo:</p>
        <p><code>usuario:x:UID:GID:comentario:/home/usuario:/bin/bash</code></p>
        <ul>
            <li><strong>UID</strong>: identificador numérico del usuario.</li>
            <li><strong>GID</strong>: identificador del grupo principal.</li>
            <li><strong>/home/usuario</strong>: directorio personal.</li>
            <li><strong>/bin/bash</strong>: shell por defecto.</li>
        </ul>
        <img width="819" height="353" alt="image" src="https://github.com/user-attachments/assets/5805d903-ca58-47eb-88a6-647319ef0a19" class="course-image"/>
        <p>En <code>/etc/group</code> están todos los grupos y los usuarios que forman parte de cada grupo.</p>
        <p>El formato típico es:</p>
        <p><code>grupo:x:GID:lista,usuarios</code></p>
        <img width="771" height="370" alt="image" src="https://github.com/user-attachments/assets/ab803637-a1d6-46e4-ab59-c69bf64950eb" class="course-image"/>
        <p>En <code>/etc/shadow</code> se encuentran las contraseñas de los usuarios cifradas. Se puede manipular el algoritmo usado en el sistema editando otro archivo de configuración (<code>/etc/login.defs</code>, PAM, etc.). También se ocupa de controlar la caducidad de las contraseñas.</p>
        <img width="806" height="372" alt="image" src="https://github.com/user-attachments/assets/1bf21b36-436b-4512-bc61-b40ab12d5d49" class="course-image"/>
        <p>En <code>/etc/gshadow</code> se gestionan las contraseñas de los grupos y también se puede ver el usuario administrador de cada grupo. Solo puede haber un único administrador por grupo; si se pone otro, se tiene que quitar el anterior.</p>
        <img width="834" height="431" alt="image" src="https://github.com/user-attachments/assets/1fefee5b-6444-4431-a69d-aa789a822610" class="course-image"/>
        <p>Aquí lo que hago es crear un usuario con <code>adduser</code>. Con este comando automáticamente se creará la carpeta <code>/home</code> del usuario, pero no los archivos dentro de la home; esos se crearán cuando inicies sesión con él (copiando el contenido de <code>/etc/skel</code>).</p>
        <img width="760" height="424" alt="image" src="https://github.com/user-attachments/assets/fb1c3fb9-91dc-4a40-81f4-ae7d782b9acc" class="course-image"/>
        <p>Cuando creas el usuario, no se crean las carpetas de la home hasta que inicies sesión con ese usuario (en ese momento se copian los ficheros por defecto).</p>
        <img width="793" height="112" alt="image" src="https://github.com/user-attachments/assets/b921cc91-ab98-4dcf-bf7c-c6535eb8de12" class="course-image"/>
        <p>Ahora hemos creado el usuario con <code>useradd</code>; este comando no crea la home y la <em>shell</em> por defecto es <code>sh</code> en vez de <code>bash</code>. Aquí lo cambiamos.</p>
        <img width="386" height="80" alt="image" src="https://github.com/user-attachments/assets/7d9c5a34-2c62-4e5e-9a59-8996416c6250" class="course-image"/>
        <p>Y ahora cambiamos la shell con el comando <code>usermod</code>.</p>
        <img width="353" height="39" alt="image" src="https://github.com/user-attachments/assets/2c84959e-dde9-4bee-95d0-8ac03ab50002" class="course-image"/>
        <p>Ahora cambiamos el dueño de la carpeta (propietario) para que coincida con el nuevo usuario.</p>
        <img width="399" height="31" alt="image" src="https://github.com/user-attachments/assets/f0e8a615-1156-4852-9587-cefd89ba2511" class="course-image"/>
        <img width="461" height="158" alt="image" src="https://github.com/user-attachments/assets/7cf8760e-73ea-410a-824e-c49f5492cee2" class="course-image"/>
        <p>Añadir un usuario a un grupo:</p>
        <img width="294" height="19" alt="image" src="https://github.com/user-attachments/assets/640ea1eb-f91c-41f8-b6f3-e287a06eaa56" class="course-image"/>
        <p>Quitar un usuario de un grupo:</p>
        <img width="302" height="50" alt="image" src="https://github.com/user-attachments/assets/7a54ec05-8c12-43ba-80ce-02f8dff1eac3" class="course-image"/>
        <p>Añadir el usuario <code>gina</code> al grupo <code>sudo</code>:</p>
        <img width="285" height="31" alt="image" src="https://github.com/user-attachments/assets/92018145-02f8-49f6-b519-5ffd0459fb93" class="course-image"/>
        <p>Al borrar un usuario, no se borra su home por defecto, para evitar pérdida de datos. Se puede borrar después manualmente.</p>
        <img width="253" height="54" alt="image" src="https://github.com/user-attachments/assets/51ff854f-80af-4f96-8915-31f55e4d0d6e" class="course-image"/>
        <p>Ahora borrando la home:</p>
        <img width="544" height="59" alt="image" src="https://github.com/user-attachments/assets/d298827a-ec8c-4018-9c54-82a2bb76d31f" class="course-image"/>
        <p>Comando para bloquear un usuario:</p>
        <img width="469" height="52" alt="image" src="https://github.com/user-attachments/assets/ab70587a-0818-42ce-ab64-991dfee08ec0" class="course-image"/>
        <p>Para desbloquear un usuario:</p>
        <img width="483" height="23" alt="image" src="https://github.com/user-attachments/assets/6e7cfcf0-c0e4-457b-8a76-0cf21de4e62d" class="course-image"/>
        <p>Modificar el nombre de un grupo y borrar el grupo:</p>
        <img width="376" height="111" alt="image" src="https://github.com/user-attachments/assets/3802a252-2b9b-472f-8192-792bcb57d0f7" class="course-image"/>
        <p>Añadir usuarios a grupos de 3 formas diferentes:</p>
        <img width="390" height="126" alt="image" src="https://github.com/user-attachments/assets/d0f20f04-a006-4dcd-b71f-c9931d32fbba" class="course-image"/>
        <p>Usuario administrador con <code>gshadow</code>:</p>
        <img width="377" height="67" alt="image" src="https://github.com/user-attachments/assets/ef26d36f-2103-462b-bdea-f0b55ecaefa4" class="course-image"/>
        <img width="477" height="147" alt="image" src="https://github.com/user-attachments/assets/ad3bbeff-a1ae-4fda-85f5-40a5394f556f" class="course-image"/>
        <p>Con este comando se modifica el grupo principal del usuario.</p>
        <img width="381" height="64" alt="image" src="https://github.com/user-attachments/assets/23413d00-c98f-427a-8b29-2dc97565e397" class="course-image"/>
        <p>Ahora hemos creado un grupo, pero le hemos editado el nombre al correcto.</p>
        <img width="541" height="126" alt="image" src="https://github.com/user-attachments/assets/6d87fb40-1866-4aa1-8d02-fdd01f92df19" class="course-image"/>
        <p>Añadiendo y eliminando usuarios de un grupo de tres formas diferentes:</p>
        <img width="634" height="379" alt="image" src="https://github.com/user-attachments/assets/be4f3977-0ed6-4df8-94ea-9cde25aeb053" class="course-image"/>
        <p>Este comando sirve para establecer el grupo principal de un usuario. Un usuario puede estar en un único grupo principal, pero en todos los grupos secundarios que quiera.</p>
        <img width="551" height="141" alt="image" src="https://github.com/user-attachments/assets/cb66469f-eca2-47bd-8c39-3f8bd52fc635" class="course-image"/>
        <p>Siempre puedes borrar grupos y a los usuarios no les pasa nada, excepto cuando ese grupo sea el principal de un usuario.</p>
        <img width="732" height="112" alt="image" src="https://github.com/user-attachments/assets/18537ae3-a8be-4aa0-a8b1-29348496d50c" class="course-image"/>
        <p>El directorio <code>/etc/skel</code> sirve para que, cuando creemos un usuario con <code>adduser</code>, se copie todo lo que hay en esa carpeta al nuevo usuario.</p>
        <img width="464" height="89" alt="image" src="https://github.com/user-attachments/assets/4f2c4795-4516-41e9-b2fd-11564dd8bae0" class="course-image"/>
        <p>Editando la home en <code>/var</code>:</p>
        <img width="680" height="231" alt="image" src="https://github.com/user-attachments/assets/cd31d6ce-4774-4ed5-b532-0dd152dcfc82" class="course-image"/>
        <p>También he editado para que la home del usuario sea <code>/k/</code>.</p>
        <img width="700" height="114" alt="image" src="https://github.com/user-attachments/assets/791d1b33-a196-42f2-8064-68b8dacf9dc3" class="course-image"/>
        <p>Aquí ponemos cada cuánto tiempo tiene que expirarle la contraseña.</p>
        <img width="727" height="208" alt="image" src="https://github.com/user-attachments/assets/3c73ea54-6f60-4fd0-af32-853f8c926365" class="course-image"/>
        <p>Ahora hacemos las comprobaciones:</p>
        <img width="739" height="243" alt="image" src="https://github.com/user-attachments/assets/7fc75702-9795-4b24-b69e-a0093f5342f9" class="course-image"/>
        <p>Ahora editamos la shell por defecto de <code>useradd</code>:</p>
        <img width="761" height="229" alt="image" src="https://github.com/user-attachments/assets/e3a3362a-5a4e-4b5e-9317-42a54425339e" class="course-image"/>
        <p>Y ahora las comprobaciones de <code>useradd</code>:</p>
        <img width="726" height="226" alt="image" src="https://github.com/user-attachments/assets/dd329b63-5e3f-40d9-9c05-71a47aa47beb" class="course-image"/>
        <p>Editando el <code>.profile</code>:</p>
        <img width="661" height="196" alt="image" src="https://github.com/user-attachments/assets/741b9a4e-9146-484b-bc11-84cdc5d27eff" class="course-image"/>
        <p>Editando el <code>.bashrc</code>:</p>
        <img width="739" height="135" alt="image" src="https://github.com/user-attachments/assets/ec0d687b-441c-4fb9-9389-4a351ea0d282" class="course-image"/>
        <p>Editando el <code>.bash_logout</code>:</p>
        <img width="698" height="135" alt="image" src="https://github.com/user-attachments/assets/d9c36819-fd4d-49cb-9472-760652c9c5c1" class="course-image"/>
        <p>Comprobaciones:</p>
        <img width="835" height="244" alt="image" src="https://github.com/user-attachments/assets/5d5f4794-2d61-4b1e-a635-d984348f8344" class="course-image"/>
        <img width="395" height="175" alt="image" src="https://github.com/user-attachments/assets/045adebb-1fcf-4771-abce-5e2b6c8635c8" class="course-image"/>
        <img width="450" height="101" alt="image" src="https://github.com/user-attachments/assets/91f63103-919c-450e-8f12-28d3a7d810bc" class="course-image"/>
        <p>Empezamos creando el usuario <code>cire</code> y el grupo <code>palomas</code>.</p>
        <p>Aquí lo que hacemos es crear dos archivos para que solo <code>root</code> pueda acceder a los archivos.</p>
        <img width="502" height="367" alt="image" src="https://github.com/user-attachments/assets/e39396ab-c519-4451-9fa8-71cc6cb5b25a" class="course-image"/>
        <p>Ahora comprobamos que al archivo y a la carpeta solo puede acceder el usuario <code>root</code>.</p>
        <img width="381" height="332" alt="image" src="https://github.com/user-attachments/assets/79282174-5a75-40a4-8f9a-671bf61c4a5b" class="course-image"/>
        <p>Ahora añadimos la excepción y lo comprobamos:</p>
        <img width="522" height="310" alt="image" src="https://github.com/user-attachments/assets/64aef155-4a2f-43a4-9199-c180bdfdf03f" class="course-image"/>
        <p>Como se puede ver, el usuario <code>blau</code> no puede acceder al archivo.</p>
        <img width="935" height="398" alt="image" src="https://github.com/user-attachments/assets/55ea14fd-b9a5-4471-a621-66865c04440d" class="course-image"/>
        <p>Ahora lo hacemos con el otro usuario, <code>roig</code>:</p>
        <img width="905" height="393" alt="image" src="https://github.com/user-attachments/assets/f9e17e4a-a51f-475c-94eb-6f18d7a6dda6" class="course-image"/>
        <p>Como se puede ver, da otro error. Esto es porque lo tenemos en <code>/var</code>, que suele tener permisos más restrictivos y políticas diferentes que <code>/home</code>.</p>
        <p>Ahora restablecemos los permisos:</p>
        <img width="550" height="332" alt="image" src="https://github.com/user-attachments/assets/74ee5060-48a5-46ad-904e-d97eb7c3bbb4" class="course-image"/>
        <p>Ahora editamos la carpeta, cambiamos sus permisos y se los quitamos:</p>
        <img width="556" height="215" alt="image" src="https://github.com/user-attachments/assets/8892850b-0600-4fef-87d3-2c4cd1c1ea20" class="course-image"/>
        <p>Para configurar la <strong>máscara</strong> en todos los usuarios:</p>
        <p>La máscara de permisos (<code>umask</code>) define qué permisos se quitan por defecto cuando se crea un archivo o carpeta. Por ejemplo, una <code>umask 022</code> hace que otros usuarios no tengan permiso de escritura por defecto.</p>
        <img width="916" height="432" alt="image" src="https://github.com/user-attachments/assets/3e497453-14a2-41c6-bbec-b665ca27a247" class="course-image"/>
        <p>Esto es para configurar la máscara para un solo usuario:</p>
        <img width="892" height="407" alt="image" src="https://github.com/user-attachments/assets/bd8cfd95-ca81-42d8-b2e5-00e19fef0ec6" class="course-image"/>
        <p>Esto es para cambiar la máscara de un usuario de forma temporal (solo en la sesión actual):</p>
        <img width="541" height="336" alt="image" src="https://github.com/user-attachments/assets/37260031-0f73-4680-9666-5a234e4992eb" class="course-image"/>
        <img width="652" height="127" alt="image" src="https://github.com/user-attachments/assets/963c8b65-395e-4383-8024-b4e816d4aba3" class="course-image"/>
        <p>Y ahora creo un usuario y entro a ese usuario y lo compruebo, creando una carpeta y un archivo para ver qué permisos se aplican automáticamente.</p>
        <img width="437" height="152" alt="image" src="https://github.com/user-attachments/assets/995078b8-ed18-428c-843e-2788ed82c586" class="course-image"/>
        <h2 class="sub">Procesos</h2>
        <p>Con <code>pstree</code> podemos ver los procesos en forma de árbol en la terminal.</p>
        <img width="959" height="887" alt="image" src="https://github.com/user-attachments/assets/c5ae73e4-d0df-4ebe-afd6-b81583fd426b" class="course-image"/>
        <p>Ahora, con <code>root</code>, podemos ver los procesos de un usuario.</p>
        <img width="748" height="207" alt="image" src="https://github.com/user-attachments/assets/6c41959e-0617-4654-99ea-21192ec97082" class="course-image"/>
        <p>Con <code>ps aux</code>, vemos todos los procesos corriendo.</p>
        <img width="906" height="390" alt="image" src="https://github.com/user-attachments/assets/ff763c6e-40ab-4a5b-a2d5-98490f3b44d6" class="course-image"/>
        <img width="347" height="62" alt="image" src="https://github.com/user-attachments/assets/16b17826-4386-4a1e-bab0-870fed29e790" class="course-image"/>
        <p>Ahora probamos con <code>htop</code> para ver los procesos.</p>
        <img width="1681" height="750" alt="image" src="https://github.com/user-attachments/assets/1e4eb66a-3593-4d9d-a0f5-a3f7123b3fd0" class="course-image"/>
        <p>Como se puede ver, <code>htop</code> es como <code>top</code>, solo que con mejor interfaz. Con <code>F4</code> podemos filtrar procesos y con <code>F9</code> matamos un proceso.</p>
        <p>También está <code>btop</code>, que para mi gusto es mucho mejor ya que te permite poder usar el ratón para interactuar con los procesos, poder monitorizar el uso de ancho de banda, la velocidad de Internet y también el uso de RAM y GPU, incluso la muestra de taseo.</p>
        <img width="252" height="48" alt="image" src="https://github.com/user-attachments/assets/7e2af435-1ed0-43c8-84c5-ce4336b2bca9" class="course-image"/>
        <img width="1672" height="756" alt="image" src="https://github.com/user-attachments/assets/403d89e8-4259-40c8-8b5d-a49f7af6bcb7" class="course-image"/>
        <p>Con <code>jobs</code> podemos ver los procesos de fondo.</p>
        <img width="217" height="65" alt="image" src="https://github.com/user-attachments/assets/3f89c329-5339-4abc-b730-f27239a606f8" class="course-image"/>
        <p>Y con <code>top &amp;</code> mandas procesos a segundo plano.</p>
        <img width="268" height="67" alt="image" src="https://github.com/user-attachments/assets/8c16f8e0-cc7a-48e3-8654-ccba518273b7" class="course-image"/>
        <img width="535" height="85" alt="image" src="https://github.com/user-attachments/assets/23ac3ed1-a152-48ba-8968-a378373186f1" class="course-image"/>
        <p>Con <code>renice</code> podemos cambiarle la prioridad a un proceso.</p>
        <p>Y con <code>nice</code> lanzas un proceso con la prioridad que tú quieras.</p>
        <p>Extra: con <code>pkill</code> puedes matar a todos los procesos por un nombre, por si se queda alguno huérfano.</p>
        <ul>
            <li>&gt;&gt; Copias de seguridad y automatización de tareas</li>
            <li>&gt;&gt; Cuotas de usuario</li>
        </ul>
    </div>
    <div class="content-section">
    <h2 class="sub">Copias de seguridad y automatización de tareas:</h2>
            <ul>
            <li>&gt;&gt; Teoria de comandes backup</li>
            <li>&gt;&gt; Practica comandas backup</li>
            <li>&gt;&gt; Practica programes backup</li>
            <li>&gt;&gt; Teoria automatizacion scripts, cron y anacron</li>
            <li>&gt;&gt; Practicas de automatizacion</li>
            </ul>
    <img width="912" height="379" alt="image" src="https://github.com/user-attachments/assets/198fab29-3c37-4989-b220-c7aa8a114ae8" class="course-image"/>
    <p>Corn y anacron son herramientas de automatizacion </p>
    <p>Difrencia entre corn y anacrin</p>
    <p>Cron ejecuta tareas programadas en una fecha y hora especifica, si el sistema esta apagado la tarea se pierde, es ideal para tareas en fechas y horas conretas y acciones especificias de un usuario,</p>
    <p>Anacron es ideal para tareas periodicas, donde no hace falta una fecha y hora especifica, normalmente se utiliza para tareas de mantenimiento del sistema y no reuiere que el sistema este inciado porque cuando se inicie sesion se ejecutara</p>
<p>Todo lo que se ponga aquí afectara a todos los users</p>
<img width="1543" height="415" alt="image" src="https://github.com/user-attachments/assets/d819d069-3fd9-4419-a377-8e3cd09b98b0" class="course-image"/>
<p>Y con esto es para hacerlo para un unico user</p>
<img width="537" height="159" alt="image" src="https://github.com/user-attachments/assets/ac35f7d6-f47d-4434-ab1c-8c919d80b86c" class="course-image"/>
<p>En este directorio todo lo que se ponga dentro automaticamente lo ejecutara crontab</p>
<img width="416" height="181" alt="image" src="https://github.com/user-attachments/assets/27e90534-7d34-4a39-b813-b92ffdbdf1de" class="course-image"/>

<img width="975" height="130" alt="image" src="https://github.com/user-attachments/assets/f1c0d7d7-fd1f-44d2-bead-c0518a55c6a6" class="course-image"/>
<img width="665" height="134" alt="image" src="https://github.com/user-attachments/assets/36b22968-fe3c-4e3e-acf4-13b822471f10" class="course-image"/>

<img width="681" height="141" alt="image" src="https://github.com/user-attachments/assets/b2f68917-01b4-42a1-b043-17be2632632a" class="course-image"/>
<img width="1013" height="248" alt="image" src="https://github.com/user-attachments/assets/f10b8998-8920-4754-aaaa-9474bfcf7c3b" class="course-image"/>

<h2 class="sub">Cuatas de disco:</h2>
<p>La cuatoa de disco es la limitacion que se le da de espacio a los usuarios de disco</p>
<p>Ahora aquí lo que hacemos es instalar el quata para hacer las limitaciones de cuaota </p>
<img width="1715" height="345" alt="image" src="https://github.com/user-attachments/assets/f5fee57a-8c13-496f-ab67-de9c76799d48" class="course-image"/>
<p>Creamos la carpeta donde van a ir los datos </p>
<img width="428" height="129" alt="image" src="https://github.com/user-attachments/assets/63bde42c-33c7-4861-a66b-a646631b34ed" class="course-image"/>
<p>Ahora comprobamos que el disco se ha montado</p>
<img width="917" height="282" alt="image" src="https://github.com/user-attachments/assets/8b218f5e-b2cc-4ec3-ac51-af6b2cd1ff21" class="course-image"/>
<img width="858" height="250" alt="image" src="https://github.com/user-attachments/assets/d90d5892-4c24-4123-9375-114ad89fbf5b" class="course-image"/>
<p>Ahora creamos un usuario de pruebas para comprobar que funciona</p>




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
    overflow-x: hidden;
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
    transition: transform 0.2s.ease, box-shadow 0.2s ease, background-color 0.2s ease;
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




