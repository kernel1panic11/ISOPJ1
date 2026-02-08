---
layout: default
title: "Sprint 3. Administración de dominios y seguridad"
---

<main class="contenedor-principal">
    <h1 class="titulo">Sprint 3. Administración de dominios y seguridad</h1>
    <div class="loading-bar"><div class="loading-progress"></div></div>
    <div class="navigation-links">
        <a href="{{ '/' | relative_url }}"><i class="fa-solid fa-house"></i> Volver al inicio</a>
        <a href="{{ '/SP3/SP3.html' | relative_url }}">Siguiente práctica <i class="fa-solid fa-arrow-right"></i></a>
    </div>

    <div class="content-section">
        <h2 class="sub">Sistema de ficheros y particiones</h2>
        <ul>
            <li>Tamaño del sector</li>
            <li>Tamaño del bloque</li>
            <li>Fragmentación interna</li>
            <li>Fragmentación externa</li>
            <li>Tipos de formateo:
                <ul>
                    <li><strong>Bajo nivel:</strong> Borra la totalidad del disco, sobrescribiendo todo con ceros e intenta reparar los sectores defectuosos. Requiere software específico del fabricante y no se realiza habitualmente desde el sistema operativo.</li>
                    <li><strong>Medio nivel:</strong> Es un punto intermedio; no borra los archivos de datos, pero marca los sectores defectuosos encontrados (sin intentar repararlos). Permite la recuperación de archivos.</li>
                    <li><strong>Alto nivel:</strong> No borra los archivos físicamente, sino que restablece el sistema de archivos (la tabla de asignación) e ignora los sectores defectuosos que pueda encontrar.</li>
                </ul>
            </li>
        </ul>
        <p></p>
    </div>

    <h2 class="sub">LDAP</h2>
    <p>Comenzamos configurando el <strong>hostname</strong>. Este será el nombre con el que la máquina será reconocida en la red.</p>
    <img width="1898" height="161" alt="Configuración hostname" src="https://github.com/user-attachments/assets/e4916ad7-1898-4624-a235-fd7b80b40e9d" class="course-image"/>
    
    <p>A continuación, editamos el archivo <code>/etc/hosts</code>:</p>
    <img width="1395" height="333" alt="Edición hosts" src="https://github.com/user-attachments/assets/d4d3b772-7efb-4a4e-acf4-e6736d01be92" class="course-image"/>
    
    <p>Posteriormente, procedemos a instalar el servicio LDAP en el servidor Ubuntu:</p>
    <img width="881" height="385" alt="Instalación LDAP" src="https://github.com/user-attachments/assets/6a68e5fc-bd14-4449-8091-d095cb3796f0" class="course-image"/>
    <img width="1919" height="1029" alt="Configuración LDAP" src="https://github.com/user-attachments/assets/33ad9380-1df9-4ec8-8043-41dceb1f5e45" class="course-image"/>
    
    <p>Tras realizar la configuración inicial, descargamos y descomprimimos la carpeta con los archivos necesarios.</p>
    <p>El directorio contiene los siguientes archivos:</p>
    <img width="749" height="32" alt="Archivos LDIF" src="https://github.com/user-attachments/assets/76fbac39-eeb4-43d6-a30f-2b11b645a936" class="course-image"/>
    
    <p>Ejecutamos el siguiente comando para reconfigurar el servidor:</p>
    <pre><code>sudo dpkg-reconfigure slapd</code></pre>
    
    <img width="1028" height="609" alt="Reconfigure Slapd 1" src="https://github.com/user-attachments/assets/a214d1f2-f598-4b84-a44d-5babb4a33abd" class="course-image"/>
    <img width="1020" height="534" alt="Reconfigure Slapd 2" src="https://github.com/user-attachments/assets/e291e457-eef2-452c-9f49-c0924ab463da" class="course-image"/>
    <img width="1010" height="323" alt="Reconfigure Slapd 3" src="https://github.com/user-attachments/assets/69d8e7ca-02e2-4659-966e-09119907bc70" class="course-image"/>

    <p>Podemos verificar que los cambios se han aplicado correctamente:</p>
    <img width="1104" height="137" alt="Verificación cambios" src="https://github.com/user-attachments/assets/acb10a28-bfc4-488d-b4a6-e464d641815c" class="course-image"/>
    <img width="1073" height="364" alt="Slapcat output" src="https://github.com/user-attachments/assets/86230fbb-0f8b-45a5-abd1-ec3ef9b76b40" class="course-image"/>

    <p>Ahora modificamos el archivo <code>uo.ldif</code> (Unidades Organizativas):</p>
    <img width="618" height="177" alt="Archivo uo.ldif" src="https://github.com/user-attachments/assets/fbedb036-5cff-4231-87a0-b65ad72547ef" class="course-image"/>

    <p>Modificamos el archivo <code>grup.ldif</code>:</p>
    <img width="679" height="235" alt="Archivo grup.ldif" src="https://github.com/user-attachments/assets/3f4453fb-edc2-4895-a496-b67ad8353e20" class="course-image"/>
    
    <p>Y finalmente, modificamos el archivo <code>usu.ldif</code>:</p>
    <img width="655" height="573" alt="Archivo usu.ldif" src="https://github.com/user-attachments/assets/cec6fb19-0e5c-4f28-8cb6-33a62c563bce" class="course-image"/>
    
    <p>Una vez configurados los archivos, ejecutamos los comandos para añadir las entradas al directorio LDAP:</p>
    <pre><code>ldapadd -c -x -D “cn=admin,dc=proves,dc=cat” -W -f uo.ldif
ldapadd -c -x -D “cn=admin,dc=proves,dc=cat” -W -f usu.ldif
ldapadd -c -x -D “cn=admin,dc=proves,dc=cat” -W -f grup.ldif</code></pre>
    
    <p>Tras ejecutar los tres comandos, debería aparecer el mensaje "adding new entry" confirmando el éxito de la operación:</p>
    <img width="684" height="577" alt="Resultado ldapadd" src="https://github.com/user-attachments/assets/5bf0626e-f2cd-4656-9ac5-f5223f54bce8" class="course-image"/>
    
    <p>En el equipo <strong>cliente</strong>, instalamos los siguientes paquetes necesarios:</p>
    <img width="539" height="37" alt="Instalación cliente" src="https://github.com/user-attachments/assets/d1247b3d-9099-4eae-89fd-23569e928d88" class="course-image"/>

    <img width="1249" height="487" alt="Configuración cliente 1" src="https://github.com/user-attachments/assets/a608fe52-1171-4b41-84e0-c85d667c1239" class="course-image" />
    <img width="954" height="307" alt="Configuración cliente 2" src="https://github.com/user-attachments/assets/f4e70792-2efe-4dc3-82cb-588d10d8f84c" class="course-image"/>
    <img width="1241" height="364" alt="Configuración cliente 3" src="https://github.com/user-attachments/assets/22abce91-a59b-4576-8d79-f8bb2a0894e6" class="course-image"/>
    <img width="1215" height="430" alt="Configuración cliente 4" src="https://github.com/user-attachments/assets/1b293e54-8430-4a0c-be7d-bd6c69899a6c" class="course-image"/>
    <img width="1185" height="382" alt="Configuración cliente 5" src="https://github.com/user-attachments/assets/d9ae6b5d-4215-41d9-bea4-9041aeefd887" class="course-image"/>
    <img width="1237" height="386" alt="Configuración cliente 6" src="https://github.com/user-attachments/assets/ce968192-daf4-428b-8d0c-0c12f2409445" class="course-image"/>
    <img width="952" height="490" alt="Configuración cliente 7" src="https://github.com/user-attachments/assets/4a2c9038-3f45-42e0-8ec1-355dce2805f8" class="course-image"/>

    <p>Editamos el archivo <code>/etc/nsswitch.conf</code>:</p>
    <img width="1230" height="608" alt="nsswitch.conf" src="https://github.com/user-attachments/assets/47b3a840-3479-46c8-8cb9-799ff882e319" class="course-image"/>
    
    <p>Al final del archivo <code>/etc/pam.d/common-session</code> (o el correspondiente según la configuración), añadimos la siguiente línea para la creación automática del directorio home:</p>
    <pre><code>session optional pam_mkhomedir.so skel=/etc/skel umask=022</code></pre>

    <h2 class="sub">Instalar interfaz gráfica</h2>
    <p>Instalamos la herramienta <strong>LDAP Account Manager (LAM)</strong>.</p>
    <p>Accedemos al administrador a través del navegador web en la dirección: <code>http://localhost/lam</code></p>
    <img width="589" height="414" alt="Login LAM" src="https://github.com/user-attachments/assets/e1151ee9-68b8-4f61-b3d5-c0ff03cb1112" class="course-image"/>

    <p>Nos dirigimos a <strong>LAM Configuration > Edit server profiles</strong>:</p>
    <img width="576" height="416" alt="Configuración LAM" src="https://github.com/user-attachments/assets/caefcec7-dfbc-4543-87e6-baa8c5484dcd" class="course-image"/>

    <h2 class="sub">Instalar SAMBA y LDAP</h2>
    <p>Para instalar Samba, ejecutamos el siguiente comando:</p>
    <pre><code>sudo apt install samba</code></pre>

    <p>Añadimos usuarios (ejemplo: azul) con el siguiente comando:</p>
    <pre><code>useradd -M -s /sbin/nologin azul</code></pre>
    <img width="560" height="109" alt="Useradd" src="https://github.com/user-attachments/assets/6017ae29-56bd-4dda-8445-d58a6bd87e23" class="course-image"/>
    
    <p>A continuación, añadimos los usuarios a Samba:</p>
    <img width="254" height="268" alt="Smbpasswd" src="https://github.com/user-attachments/assets/1f482bda-c2b9-47c9-8f29-e015b2c045e5" class="course-image"/>
    
    <p>Editamos el archivo de configuración con <code>nano /etc/samba/smb.conf</code> y reiniciamos el servicio:</p>
    <img width="987" height="583" alt="Configuración Samba" src="https://github.com/user-attachments/assets/6bd98594-e56a-4096-a5b5-4392becafd34" class="course-image"/>
    
    <p>Para comprobar la conexión, podemos utilizar <code>smbclient</code>.</p>

    <h2 class="sub">LDAP y SMB (Integración)</h2>
    <p>Para realizar la integración, instalamos los paquetes necesarios:</p>
    <pre><code>sudo apt install libnss-ldap libpam-ldap smbldap-tools -y</code></pre>

    <p>Editamos <code>nano /etc/samba/smb.conf</code> e incluimos lo siguiente dentro del bloque <code>[global]</code>:</p>
    <pre><code># Configuració Backend LDAP
passdb backend = ldapsam:ldap://localhost
ldap suffix = dc=proves,dc=cat
ldap user suffix = ou=usuaris
ldap group suffix = ou=grups
# DN de l'administrador que has configurat abans
ldap admin dn = cn=admin,dc=proves,dc=cat
ldap ssl = no</code></pre>

    <p>Creamos el recurso compartido editando nuevamente el archivo <code>/etc/samba/smb.conf</code> y añadiendo:</p>
    <pre><code>[recurs_alu1]
comment = Carpeta privada alu1 
path = /srv/samba/alu1
browseable = yes
read only = no
guest ok = no
valid users = alu1
writable = yes
create mask = 0770
directory mask = 0770</code></pre>
    
    <img width="960" height="532" alt="Samba Share Conf" src="https://github.com/user-attachments/assets/caa42f7f-3e4b-471b-abc3-acd60368b82e" class="course-image"/>

    <p>Aplicamos los cambios creando el directorio, asignando permisos y reiniciando servicios:</p>
    <pre><code>mkdir -p /srv/samba/alu1
chmod 777 /srv/samba/alu1
systemctl restart smbd nmbd</code></pre>

    <p>Para verificar el funcionamiento:</p>
    <ol>
        <li>Desde el equipo cliente (asegurando conectividad previa), abrimos el navegador de archivos.</li>
        <li>Vamos a “Otras ubicaciones” e introducimos: <code>smb://10.0.2.15/recurs_alu1</code>.</li>
        <li>Se solicitará autenticación. Introducimos:
            <ul>
                <li><strong>Usuario:</strong> alu1</li>
                <li><strong>Contraseña:</strong> (la definida en el archivo <code>usu.ldif</code>).</li>
            </ul>
        </li>
        <li>Si la configuración es correcta, tendremos acceso para crear y editar carpetas.</li>
    </ol>

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

/* Estilo adicional para bloques de código */
pre {
    background-color: #282c34;
    padding: 1rem;
    border-radius: 6px;
    overflow-x: auto;
    margin: 1rem 0;
}

pre code {
    background-color: transparent;
    padding: 0;
    color: #abb2bf;
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
