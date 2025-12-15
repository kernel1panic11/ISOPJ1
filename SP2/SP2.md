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
                    <li><strong>Bajo nivel</strong>: lo que hace es borrar todo; sobrescribe todo con ceros e intenta reparar los sectores defectuosos, pero se necesitan programas específicos; no se puede hacer a través del sistema operativo de forma normal.</li>
                    <li><strong>Medio nivel</strong>: sería un intermedio, no borra archivos; los sectores defectuosos que encuentra los marca, pero no intenta arreglarlos. También se pueden recuperar los archivos.</li>
                    <li><strong>Alto nivel</strong>: no borra los archivos, solo borra el sistema de archivos (la tabla de asignación) e ignora los sectores defectuosos que pueda encontrar.</li>
                </ul>
            </li>
        </ul>
        <p>En la práctica, hoy en día se suele hablar más de <strong>formateo rápido</strong> (solo recrea las estructuras del sistema de archivos) y <strong>formateo completo</strong> (recorre el disco y marca sectores defectuosos). El “bajo nivel” real lo hace el fabricante del disco; las herramientas que usamos simulan este comportamiento escribiendo en toda la superficie.</p>
        <p>El tamaño del <strong>sector</strong> lo fija el fabricante del disco (por ejemplo, 512 B o 4 KB físicos), mientras que el tamaño del <strong>bloque</strong> o clúster se define al crear el sistema de archivos. Un tamaño de bloque más grande reduce las tablas de asignación, pero desperdicia más espacio (más fragmentación interna). Un tamaño de bloque más pequeño aprovecha mejor el espacio, pero aumenta el tamaño de las tablas y el número de accesos.</p>
        <p>La <strong>fragmentación interna</strong> es el espacio que se pierde dentro de un bloque cuando el archivo no lo llena entero (por ejemplo, un archivo de 17 B en un bloque de 4096 B desperdicia 4079 B). La <strong>fragmentación externa</strong> ocurre cuando un archivo está repartido en muchos trozos no contiguos en el disco, lo que empeora el rendimiento al leerlo.</p>
    </div>

    <div class="content-section">
        <h2 class="sub">Gestión de particiones:</h2>
        <p>Se puede hacer de dos formas usando:</p>
        <ul>
            <li><strong>GParted</strong> (Interfaz gráfica)</li>
            <li><strong>Comandos</strong> (por ejemplo, <code>fdisk</code>, <code>parted</code> o <code>lsblk</code>)</li>
        </ul>
        <p>Aunque, como he mencionado antes, hay ciertas partes que se tienen que hacer por terminal, ya que por GUI no se puede (por ejemplo, algunas opciones específicas de <code>mkfs</code>, trabajar por SSH, scripts de automatización, etc.).</p>
        <p>Además, es importante entender que el disco puede estar particionado con <strong>MBR</strong> (tablas de particiones antiguas, máximo 4 particiones primarias) o con <strong>GPT</strong> (más moderno, soporta muchas más particiones y discos de mayor tamaño). Herramientas como <code>gdisk</code> o <code>parted</code> detectan este esquema.</p>
        
        
    </div>

    <div>
        <p>Para empezar, crearemos un disco virtual nuevo en VMware. Lo normal es crear un disco SATA, pero en mi caso he indicado NVMe, ya que así va más <strong>rápido</strong> (menos latencia y mejor rendimiento simulado). Como se puede ver, el disco duro es de 10 GB.</p>
        <img width="826" height="670" alt="Creación de disco en VMware" src="https://github.com/user-attachments/assets/d2fce307-de10-4ac3-80e6-8d1ea669cd9e" class="course-image"/>
        
        <p>Ahora comprobamos el tamaño del sector del nuevo disco sin formatear con <code>fdisk -l</code>. Como se puede ver en la salida, el sector es de 512 bytes.</p>
        <img width="933" height="569" alt="Comando fdisk -l" src="https://github.com/user-attachments/assets/ff7b4574-eb1e-48bc-a71a-11da2425f37f" class="course-image"/>
        
        <p>A continuación, con el comando <code>tune2fs -l (disco)</code>, podemos ver el tamaño del bloque de una partición ya formateada con <code>ext4</code>.</p>
        <img width="621" height="128" alt="Comando tune2fs" src="https://github.com/user-attachments/assets/a3a3b284-07db-4318-9f51-bd2eb2005c3d" class="course-image"/>
        
        <p>Con <code>df -Th</code>, podemos ver el sistema de archivos que se usa, el espacio ocupado y el libre disponible. El parámetro <code>-h</code> es opcional, pero recomendable porque lo pone en formato “human-readable”; representa los valores en GB/MB, siendo así más fácil de interpretar.</p>
        <img width="699" height="180" alt="Comando df -Th" src="https://github.com/user-attachments/assets/ca0866b5-6e8f-4668-b300-59c24d8ac9a7" class="course-image"/>
        
        <p>La medida de clúster (Windows) y de bloque (Linux) es la unidad mínima lógica donde se guardan los datos a nivel de sistema operativo. Por defecto son 4096 bytes (igual a 8 sectores) y esta medida sí se puede cambiar al formatear la partición. Cada partición del disco puede tener un tamaño de bloque y un sistema de archivos diferentes.</p>
        
        <p>Este es un claro ejemplo de <strong>fragmentación interna</strong>: podemos ver cómo el archivo pesa realmente 17 bytes, pero ocupa 4 KB (o el tamaño mínimo de bloque) en el disco; esa diferencia es el espacio perdido.</p>
        <img width="682" height="191" alt="Fragmentación interna" src="https://github.com/user-attachments/assets/04eca4eb-684d-45af-8bc4-ec36fbfd739c" class="course-image"/>
        
        <p><strong>Particiones</strong>: una <strong>partición</strong> es un trozo físico (contiguo) del disco duro, definido en la tabla de particiones. No podemos modificar la medida del bloque directamente en la partición en crudo; eso se hace al formatear con el sistema de archivos. Un <strong>volumen</strong> es una capa de abstracción superior (por ejemplo, LVM, RAID, etc.).</p>
        
        <p>GParted sirve para gestionar particiones de forma visual (crear, borrar, redimensionar, cambiar el tipo de sistema de archivos, ver flags como <code>boot</code>, etc.).</p>
        <img width="777" height="532" alt="Interfaz GParted" src="https://github.com/user-attachments/assets/53f4f163-3724-4527-8ade-8cdf99407de5" class="course-image"/>
        
        <p>Aquí hemos creado dos particiones de aproximadamente 5 GB cada una utilizando la terminal. Como se puede ver en la imagen, las dos particiones se han creado correctamente.</p>
        <img width="931" height="761" alt="Creación particiones terminal" src="https://github.com/user-attachments/assets/d17a2000-1cb1-48ef-9594-029f21bfeba1" class="course-image"/>
        
        <p>Aquí he dado formato a la primera partición en <strong>ext4</strong>, usando el comando <code>mkfs.ext4 -b 2048 /dev/nvme0n1p1</code>. El parámetro <code>-b 2048</code> fuerza un tamaño de bloque de 2048 bytes en lugar de los 4096 habituales.</p>
        <img width="893" height="294" alt="Formato ext4" src="https://github.com/user-attachments/assets/8f70f76c-b256-4adf-b9ed-ca11e9f471be" class="course-image"/>
        
        <p>Seguidamente, he dado formato a la partición secundaria en <strong>NTFS</strong>, usando el comando <code>mkfs.ntfs /dev/nvme0n1p1</code> (OJO: Asegúrate de que apuntas a la partición correcta, p2). En este caso ha tardado más, ya que a veces NTFS realiza un llenado de ceros o comprobación inicial más exhaustiva.</p>
        <p><strong>Nota importante</strong>: Es vital fijarse bien en el dispositivo (ej. <code>/dev/nvme0n1p2</code>), porque si te equivocas puedes borrar datos de otra partición.</p>
        <img width="563" height="134" alt="Formato NTFS" src="https://github.com/user-attachments/assets/9ffc4cf6-337d-4a5a-b2e7-be66dabf4a9e" class="course-image"/>
        
        <p>Comprobando por terminal que se han creado bien las particiones:</p>
        <img width="1039" height="201" alt="lsblk comprobación" src="https://github.com/user-attachments/assets/e6fbdcd9-fe1a-4280-b15b-b4c410f46e85" class="course-image"/>
        
        <p>Y ahora lo verificamos visualmente con GParted:</p>
        <img width="771" height="257" alt="GParted verificación" src="https://github.com/user-attachments/assets/10f5d46d-f9bc-47cc-bfe4-5b89e0a004dc" class="course-image"/>
    </div>

    <div>
        <h2 class="sub">Montaje de particiones:</h2>
        
        <p><strong>Montaje de partición en modo temporal</strong></p>
        <p>Para empezar, montaremos de forma temporal una partición en <code>/mnt/p1</code>. Usaremos el comando <code>mount -t ext4 /dev/nvme0n1p1 /mnt/p1</code>. Dentro de la partición recién montada, crearé una carpeta llamada “test”. Si reiniciamos la VM, comprobaremos que la partición ya no está montada.</p>
        <p>Esto ocurre porque los montajes hechos con el comando <code>mount</code> son <strong>temporales</strong> (en memoria) y no persisten tras el reinicio si no se configuran en <code>/etc/fstab</code>.</p>
        <p><em>Nota:</em> Incluir la opción <code>-t ext4</code> especifica explícitamente el sistema de archivos, ayudando al sistema a montarlo correctamente sin tener que "adivinar" el tipo.</p>
        <img width="624" height="144" alt="Comando mount" src="https://github.com/user-attachments/assets/56fe0548-4eb7-4d4f-a550-8653953e5818" class="course-image"/>
        <img width="338" height="31" alt="Creación carpeta test" src="https://github.com/user-attachments/assets/8cabda37-5690-4f65-8d4a-57bae83564c5" class="course-image"/>
        
        <p><strong>Montaje de partición en modo permanente:</strong></p>
        <p>Para que el montaje sea persistente, editamos el archivo <code>/etc/fstab</code> y añadimos una línea con el UUID (o dispositivo), el punto de montaje, el sistema de archivos (ext4) y las opciones (defaults). Así, el sistema la montará automáticamente en el arranque.</p>
        <img width="808" height="256" alt="Edición fstab" src="https://github.com/user-attachments/assets/93f4df08-d910-4f46-9f6b-04daf5424912" class="course-image"/>
        
        <p><strong>Comprobación después del reinicio:</strong></p>
        <p>Tras reiniciar, comprobamos que la partición se ha montado sola y que la carpeta “test” sigue ahí.</p>
        <img width="820" height="179" alt="Comprobación tras reinicio" src="https://github.com/user-attachments/assets/732511da-07a3-4228-ac73-ee2b77ba7ca0" class="course-image"/>
        
        <p><strong>Desfragmentación</strong></p>
        <p>En <strong>NTFS</strong> (Windows) la desfragmentación es crucial. En Linux (ext4), la fragmentación es mucho menor por diseño, pero existen herramientas como <code>e4defrag</code> para analizar y optimizar si fuera necesario.</p>
        <img width="1288" height="514" alt="Desfragmentación" src="https://github.com/user-attachments/assets/f9e8f8d2-03d9-4d99-8342-e5669b44f21f" class="course-image"/>
    </div>

    <div class="content-section">
        <h2 class="sub">Gestión de procesos</h2>
        <p>En Linux, los procesos se pueden gestionar con varios comandos:</p>
        <ul>
            <li><code>ps aux</code>: muestra la lista de todos los procesos en ejecución con detalles (usuario, PID, CPU, memoria, etc.).</li>
            <li><code>top</code> o <code>htop</code>: muestran procesos en tiempo real, permitiendo ordenar por consumo de recursos.</li>
            <li><code>kill PID</code>: envía una señal de terminación (SIGTERM - 15) para cerrar el proceso ordenadamente.</li>
            <li><code>kill -9 PID</code>: fuerza el cierre inmediato (SIGKILL) si el proceso no responde.</li>
            <li><code>nice</code> y <code>renice</code>: ajustan la prioridad de ejecución (valores de -20 a 19).</li>
        </ul>
        <p>También es útil <code>systemctl</code> para gestionar servicios (daemons) del sistema.</p>
    </div>

    <div class="content-section">
        <h2 class="sub">Gestión de usuarios, grupos y permisos</h2>
        <p>El archivo <code>/etc/passwd</code> contiene la información de los usuarios. El formato es:</p>
        <p><code>usuario:x:UID:GID:comentario:/home/usuario:/bin/bash</code></p>
        <img width="819" height="353" alt="Archivo passwd" src="https://github.com/user-attachments/assets/5805d903-ca58-47eb-88a6-647319ef0a19" class="course-image"/>
        
        <p>En <code>/etc/group</code> se definen los grupos y sus miembros.</p>
        <img width="771" height="370" alt="Archivo group" src="https://github.com/user-attachments/assets/ab803637-a1d6-46e4-ab59-c69bf64950eb" class="course-image"/>
        
        <p>En <code>/etc/shadow</code> están las contraseñas cifradas y la información de caducidad. Solo root puede leerlo.</p>
        <img width="806" height="372" alt="Archivo shadow" src="https://github.com/user-attachments/assets/1bf21b36-436b-4512-bc61-b40ab12d5d49" class="course-image"/>
        
        <p>En <code>/etc/gshadow</code> se gestionan las contraseñas y administradores de grupos.</p>
        <img width="834" height="431" alt="Archivo gshadow" src="https://github.com/user-attachments/assets/1fefee5b-6444-4431-a69d-aa789a822610" class="course-image"/>
        
        <p>Creación de usuario con <code>adduser</code> (interactivo, crea home y copia <code>/etc/skel</code>):</p>
        <img width="760" height="424" alt="Comando adduser" src="https://github.com/user-attachments/assets/fb1c3fb9-91dc-4a40-81f4-ae7d782b9acc" class="course-image"/>
        <img width="793" height="112" alt="Verificación home" src="https://github.com/user-attachments/assets/b921cc91-ab98-4dcf-bf7c-c6535eb8de12" class="course-image"/>
        
        <p>Creación con <code>useradd</code> (más manual, por defecto sin home y con shell <code>sh</code>):</p>
        <img width="386" height="80" alt="Comando useradd" src="https://github.com/user-attachments/assets/7d9c5a34-2c62-4e5e-9a59-8996416c6250" class="course-image"/>
        
        <p>Cambiando la shell a bash con <code>usermod -s</code>:</p>
        <img width="353" height="39" alt="Cambio shell" src="https://github.com/user-attachments/assets/2c84959e-dde9-4bee-95d0-8ac03ab50002" class="course-image"/>
        
        <p>Cambiando el propietario de una carpeta con <code>chown</code>:</p>
        <img width="399" height="31" alt="Comando chown" src="https://github.com/user-attachments/assets/f0e8a615-1156-4852-9587-cefd89ba2511" class="course-image"/>
        <img width="461" height="158" alt="Verificación chown" src="https://github.com/user-attachments/assets/7cf8760e-73ea-410a-824e-c49f5492cee2" class="course-image"/>
        
        <p>Gestión de grupos (añadir/quitar usuarios):</p>
        <img width="294" height="19" alt="Añadir usuario a grupo" src="https://github.com/user-attachments/assets/640ea1eb-f91c-41f8-b6f3-e287a06eaa56" class="course-image"/>
        <img width="302" height="50" alt="Quitar usuario de grupo" src="https://github.com/user-attachments/assets/7a54ec05-8c12-43ba-80ce-02f8dff1eac3" class="course-image"/>
        
        <p>Otorgar permisos de administrador añadiendo al grupo <code>sudo</code>:</p>
        <img width="285" height="31" alt="Grupo sudo" src="https://github.com/user-attachments/assets/92018145-02f8-49f6-b519-5ffd0459fb93" class="course-image"/>
        
        <p>Eliminación de usuarios (con y sin borrar la home):</p>
        <img width="253" height="54" alt="Borrar usuario" src="https://github.com/user-attachments/assets/51ff854f-80af-4f96-8915-31f55e4d0d6e" class="course-image"/>
        <img width="544" height="59" alt="Borrar home manual" src="https://github.com/user-attachments/assets/d298827a-ec8c-4018-9c54-82a2bb76d31f" class="course-image"/>
        
        <p>Bloquear (<code>passwd -l</code>) y desbloquear (<code>passwd -u</code>) usuarios:</p>
        <img width="469" height="52" alt="Bloquear usuario" src="https://github.com/user-attachments/assets/ab70587a-0818-42ce-ab64-991dfee08ec0" class="course-image"/>
        <img width="483" height="23" alt="Desbloquear usuario" src="https://github.com/user-attachments/assets/6e7cfcf0-c0e4-457b-8a76-0cf21de4e62d" class="course-image"/>
        
        <p>Modificar y borrar grupos (<code>groupmod</code>, <code>groupdel</code>):</p>
        <img width="376" height="111" alt="Modificar grupo" src="https://github.com/user-attachments/assets/3802a252-2b9b-472f-8192-792bcb57d0f7" class="course-image"/>
        <img width="390" height="126" alt="Añadir a grupos" src="https://github.com/user-attachments/assets/d0f20f04-a006-4dcd-b71f-c9931d32fbba" class="course-image"/>
        
        <p>Administradores de grupo con <code>gpasswd</code>:</p>
        <img width="377" height="67" alt="gpasswd admin" src="https://github.com/user-attachments/assets/ef26d36f-2103-462b-bdea-f0b55ecaefa4" class="course-image"/>
        <img width="477" height="147" alt="Verificación gpasswd" src="https://github.com/user-attachments/assets/ad3bbeff-a1ae-4fda-85f5-40a5394f556f" class="course-image"/>
        
        <p>Modificando el grupo principal con <code>usermod -g</code>:</p>
        <img width="381" height="64" alt="Grupo principal" src="https://github.com/user-attachments/assets/23413d00-c98f-427a-8b29-2dc97565e397" class="course-image"/>
        <img width="541" height="126" alt="Corrección grupo" src="https://github.com/user-attachments/assets/6d87fb40-1866-4aa1-8d02-fdd01f92df19" class="course-image"/>
        
        <p>El directorio <code>/etc/skel</code> (Skeleton) contiene los archivos base que se copian a la home de cada nuevo usuario creado.</p>
        <img width="464" height="89" alt="Directorio skel" src="https://github.com/user-attachments/assets/4f2c4795-4516-41e9-b2fd-11564dd8bae0" class="course-image"/>
        
        <p>Personalización avanzada de usuarios (cambio de directorio home, expiración de contraseña, configuración de bashrc, etc.):</p>
        <img width="680" height="231" alt="Home en var" src="https://github.com/user-attachments/assets/cd31d6ce-4774-4ed5-b532-0dd152dcfc82" class="course-image"/>
        <img width="700" height="114" alt="Home personalizada" src="https://github.com/user-attachments/assets/791d1b33-a196-42f2-8064-68b8dacf9dc3" class="course-image"/>
        <img width="727" height="208" alt="Expiración contraseña" src="https://github.com/user-attachments/assets/3c73ea54-6f60-4fd0-af32-853f8c926365" class="course-image"/>
        <img width="739" height="243" alt="Comprobaciones usuario" src="https://github.com/user-attachments/assets/7fc75702-9795-4b24-b69e-a0093f5342f9" class="course-image"/>
        <img width="761" height="229" alt="Shell useradd default" src="https://github.com/user-attachments/assets/e3a3362a-5a4e-4b5e-9317-42a54425339e" class="course-image"/>
        <img width="726" height="226" alt="Comprobación defaults" src="https://github.com/user-attachments/assets/dd329b63-5e3f-40d9-9c05-71a47aa47beb" class="course-image"/>
        <img width="661" height="196" alt="Editar .profile" src="https://github.com/user-attachments/assets/741b9a4e-9146-484b-bc11-84cdc5d27eff" class="course-image"/>
        <img width="739" height="135" alt="Editar .bashrc" src="https://github.com/user-attachments/assets/ec0d687b-441c-4fb9-9389-4a351ea0d282" class="course-image"/>
        <img width="698" height="135" alt="Editar .bash_logout" src="https://github.com/user-attachments/assets/d9c36819-fd4d-49cb-9472-760652c9c5c1" class="course-image"/>
        <img width="835" height="244" alt="Pruebas configuración" src="https://github.com/user-attachments/assets/5d5f4794-2d61-4b1e-a635-d984348f8344" class="course-image"/>
        <img width="395" height="175" alt="Más pruebas" src="https://github.com/user-attachments/assets/045adebb-1fcf-4771-abce-5e2b6c8635c8" class="course-image"/>
        <img width="450" height="101" alt="Más pruebas 2" src="https://github.com/user-attachments/assets/91f63103-919c-450e-8f12-28d3a7d810bc" class="course-image"/>

        <p><strong>Permisos y ACLs:</strong></p>
        <p>Creación de usuario <code>cire</code> y grupo <code>palomas</code>. Pruebas de restricción de acceso con <code>chmod</code> y ACLs.</p>
        <img width="502" height="367" alt="Permisos root" src="https://github.com/user-attachments/assets/e39396ab-c519-4451-9fa8-71cc6cb5b25a" class="course-image"/>
        <img width="381" height="332" alt="Comprobación root" src="https://github.com/user-attachments/assets/79282174-5a75-40a4-8f9a-671bf61c4a5b" class="course-image"/>
        <img width="522" height="310" alt="Excepciones ACL" src="https://github.com/user-attachments/assets/64aef155-4a2f-43a4-9199-c180bdfdf03f" class="course-image"/>
        <img width="935" height="398" alt="Fallo acceso blau" src="https://github.com/user-attachments/assets/55ea14fd-b9a5-4471-a621-66865c04440d" class="course-image"/>
        <img width="905" height="393" alt="Fallo acceso roig" src="https://github.com/user-attachments/assets/f9e17e4a-a51f-475c-94eb-6f18d7a6dda6" class="course-image"/>
        
        <p>Restablecimiento de permisos y uso de <code>umask</code>:</p>
        <p>La máscara de permisos (<code>umask</code>) define qué permisos se quitan por defecto al crear un archivo. Por ejemplo, <code>umask 022</code> quita permisos de escritura a "grupo" y "otros".</p>
        <img width="550" height="332" alt="Restablecer permisos" src="https://github.com/user-attachments/assets/74ee5060-48a5-46ad-904e-d97eb7c3bbb4" class="course-image"/>
        <img width="556" height="215" alt="Editar carpeta" src="https://github.com/user-attachments/assets/8892850b-0600-4fef-87d3-2c4cd1c1ea20" class="course-image"/>
        <img width="916" height="432" alt="Configurar umask global" src="https://github.com/user-attachments/assets/3e497453-14a2-41c6-bbec-b665ca27a247" class="course-image"/>
        <img width="892" height="407" alt="Umask usuario" src="https://github.com/user-attachments/assets/bd8cfd95-ca81-42d8-b2e5-00e19fef0ec6" class="course-image"/>
        <img width="541" height="336" alt="Umask temporal" src="https://github.com/user-attachments/assets/37260031-0f73-4680-9666-5a234e4992eb" class="course-image"/>
        <img width="652" height="127" alt="Verificación umask" src="https://github.com/user-attachments/assets/963c8b65-395e-4383-8024-b4e816d4aba3" class="course-image"/>
        <img width="437" height="152" alt="Prueba creación archivo" src="https://github.com/user-attachments/assets/995078b8-ed18-428c-843e-2788ed82c586" class="course-image"/>

        <h2 class="sub">Procesos</h2>
        <p>Con <code>pstree</code> podemos ver los procesos en forma de árbol jerárquico.</p>
        <img width="959" height="887" alt="pstree" src="https://github.com/user-attachments/assets/c5ae73e4-d0df-4ebe-afd6-b81583fd426b" class="course-image"/>
        
        <p>Visualización de procesos con <code>ps</code> y filtros:</p>
        <img width="748" height="207" alt="Procesos usuario" src="https://github.com/user-attachments/assets/6c41959e-0617-4654-99ea-21192ec97082" class="course-image"/>
        <img width="906" height="390" alt="ps aux" src="https://github.com/user-attachments/assets/ff763c6e-40ab-4a5b-a2d5-98490f3b44d6" class="course-image"/>
        <img width="347" height="62" alt="Filtro ps" src="https://github.com/user-attachments/assets/16b17826-4386-4a1e-bab0-870fed29e790" class="course-image"/>
        
        <p>Monitorización con <code>htop</code> (mejor interfaz que <code>top</code>, permite filtrar con F4 y matar procesos con F9):</p>
        <img width="1681" height="750" alt="htop" src="https://github.com/user-attachments/assets/1e4eb66a-3593-4d9d-a0f5-a3f7123b3fd0" class="course-image"/>
        
        <p>Uso de <code>btop</code>. Esta herramienta es muy potente visualmente; permite usar el ratón, ver el ancho de banda, temperaturas, y gráficos detallados de CPU/RAM.</p>
        <img width="252" height="48" alt="Instalar btop" src="https://github.com/user-attachments/assets/7e2af435-1ed0-43c8-84c5-ce4336b2bca9" class="course-image"/>
        <img width="1672" height="756" alt="Interfaz btop" src="https://github.com/user-attachments/assets/403d89e8-4259-40c8-8b5d-a49f7af6bcb7" class="course-image"/>
        
        <p>Gestión de trabajos en segundo plano (<code>jobs</code>, <code>&</code>, <code>bg</code>, <code>fg</code>) y prioridades (<code>nice</code>, <code>renice</code>):</p>
        <img width="217" height="65" alt="jobs" src="https://github.com/user-attachments/assets/3f89c329-5339-4abc-b730-f27239a606f8" class="course-image"/>
        <img width="268" height="67" alt="segundo plano" src="https://github.com/user-attachments/assets/8c16f8e0-cc7a-48e3-8654-ccba518273b7" class="course-image"/>
        <img width="535" height="85" alt="kill process" src="https://github.com/user-attachments/assets/23ac3ed1-a152-48ba-8968-a378373186f1" class="course-image"/>
    </div>

    <div class="content-section">
        <h2 class="sub">Copias de seguridad y automatización de tareas</h2>
        <ul>
            <li>Teoría de comandos de backup (tar, rsync)</li>
            <li>Práctica de comandos de backup</li>
            <li>Práctica de programas de backup</li>
            <li>Teoría automatización: scripts, Cron y Anacron</li>
            <li>Prácticas de automatización</li>
        </ul>
        <img width="912" height="379" alt="Esquema backup" src="https://github.com/user-attachments/assets/198fab29-3c37-4989-b220-c7aa8a114ae8" class="course-image"/>

        <h3>Diferencia entre Cron y Anacron</h3>
        <p><strong>Cron</strong> y <strong>Anacron</strong> son las herramientas principales de automatización en Linux:</p>
        
        
        
        <ul>
            <li><strong>Cron:</strong> Ejecuta tareas programadas en una fecha y hora exactas (ej: "cada lunes a las 3:00 AM"). Es ideal para servidores que están encendidos 24/7. Si el sistema está apagado a la hora programada, la tarea <strong>se pierde</strong> y no se ejecuta hasta el siguiente ciclo.</li>
            <li><strong>Anacron:</strong> Está diseñado para tareas periódicas (diarias, semanales, mensuales) en sistemas que no están siempre encendidos (como portátiles o PCs de escritorio). Si el sistema estaba apagado, Anacron ejecutará la tarea pendiente poco después de arrancar el equipo.</li>
        </ul>

        <p>Configuración del archivo <code>/etc/crontab</code> (afecta a todo el sistema):</p>
        <img width="1543" height="415" alt="etc crontab" src="https://github.com/user-attachments/assets/d819d069-3fd9-4419-a377-8e3cd09b98b0" class="course-image"/>

        <p>Para configurar tareas de un usuario específico se usa <code>crontab -e</code>:</p>
        <img width="537" height="159" alt="crontab -e" src="https://github.com/user-attachments/assets/ac35f7d6-f47d-4434-ab1c-8c919d80b86c" class="course-image"/>

        <p>El directorio <code>/etc/cron.d/</code> permite añadir ficheros de configuración adicionales que Cron leerá automáticamente:</p>
        <img width="416" height="181" alt="cron.d" src="https://github.com/user-attachments/assets/27e90534-7d34-4a39-b813-b92ffdbdf1de" class="course-image"/>

        <p>Pruebas de ejecución y logs de cron:</p>
        <img width="975" height="130" alt="Logs cron" src="https://github.com/user-attachments/assets/f1c0d7d7-fd1f-44d2-bead-c0518a55c6a6" class="course-image"/>
        <img width="665" height="134" alt="Verificación ejecución" src="https://github.com/user-attachments/assets/36b22968-fe3c-4e3e-acf4-13b822471f10" class="course-image"/>
        <img width="681" height="141" alt="Más logs" src="https://github.com/user-attachments/assets/b2f68917-01b4-42a1-b043-17be2632632a" class="course-image"/>
        <img width="1013" height="248" alt="Comprobación final" src="https://github.com/user-attachments/assets/f10b8998-8920-4754-aaaa-9474bfcf7c3b" class="course-image"/>

        <h2 class="sub">Cuotas de disco</h2>
        <p>Las <strong>cuotas de disco</strong> permiten limitar el espacio de almacenamiento o el número de inodos (archivos) que un usuario o grupo puede utilizar en una partición.</p>
        
        <p>Primero, instalamos el paquete <code>quota</code> para gestionar las limitaciones:</p>
        <img width="1715" height="345" alt="Instalar quota" src="https://github.com/user-attachments/assets/f5fee57a-8c13-496f-ab67-de9c76799d48" class="course-image"/>
        
        <p>Preparamos el punto de montaje y la carpeta de datos:</p>
        <img width="428" height="129" alt="Carpeta datos" src="https://github.com/user-attachments/assets/63bde42c-33c7-4861-a66b-a646631b34ed" class="course-image"/>
        
        <p>Comprobamos que el disco se ha montado correctamente con las opciones necesarias (normalmente <code>usrquota</code> y/o <code>grpquota</code> en <code>/etc/fstab</code>):</p>
        <img width="917" height="282" alt="Mount quota" src="https://github.com/user-attachments/assets/8b218f5e-b2cc-4ec3-ac51-af6b2cd1ff21" class="course-image"/>
        <img width="858" height="250" alt="Verificación fstab quota" src="https://github.com/user-attachments/assets/d90d5892-4c24-4123-9375-114ad89fbf5b" class="course-image"/>
        
        <p>Finalmente, creamos un usuario de pruebas y verificamos que la limitación funciona intentando escribir más datos de los permitidos:</p>
        <img width="1113" height="215" alt="Usuario prueba quota" src="https://github.com/user-attachments/assets/c6dbbb09-2dea-47bf-8e04-8ee48391e6db" class="course-image"/>
        <img width="1725" height="275" alt="Test llenado disco" src="https://github.com/user-attachments/assets/68a1d122-4ae6-4afa-85c9-f2b193e2006f" class="course-image"/>
        <img width="760" height="156" alt="Error espacio quota" src="https://github.com/user-attachments/assets/8f69e6db-1020-4c51-81e7-2c1a4e5aee7d" class="course-image"/>
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
