---
layout: default
title: "Sprint 5. Logs y monitorización"
---

<style>
:root { --bg-image: url('{{ "/assetscss/pract22.gif" | relative_url }}'); }
</style>

<main class="contenedor-principal">
    <h1 class="titulo">Sprint 5. Logs y monitorización</h1>
    <div class="loading-bar"><div class="loading-progress"></div></div>
    <div class="content-section">
        <h2 class="sub">Servidor de actualizaciones Ubuntu (CDN)</h2>
        
        <p>Empezamos instalando el <code>apache2</code>:</p>
        <div class="img-wrap">
            <img src="https://github.com/user-attachments/assets/a4b43d1e-6732-43f2-9155-83a3a02b3c9c" alt="Apache">
        </div>

        <p>Después instalamos el <code>apt-mirror</code>:</p>
        <div class="img-wrap">
            <img src="https://github.com/user-attachments/assets/55e4665d-e127-4ae7-a1c9-d148b55c5c1f" alt="Apt-mirror">
        </div>

        <p>En este archivo ponemos los repositorios que el servidor debe descargar:</p>
        <div class="img-wrap">
            <img src="https://github.com/user-attachments/assets/0302cb80-cd2b-4e03-b290-1eb47eac7ef9" alt="Configuración">
        </div>

        <p>Descomentamos las repos y dejamos solo la de Chrome para ganar tiempo:</p>
        <div class="img-wrap">
            <img src="https://github.com/user-attachments/assets/494e569b-4820-4c51-91bc-fdaf366c070e" alt="Repositorios">
        </div>

        <p>Ejecutamos <code>apt-mirror</code> para descargar los paquetes del repositorio:</p>
        <div class="img-wrap">
            <img src="https://github.com/user-attachments/assets/d78fde5f-855d-4908-90cc-f5327e3b80d2" alt="Ejecución">
        </div>

        <p>Configuramos Apache y creamos el <strong>softlink</strong>:</p>
        <div class="img-wrap">
            <img src="https://github.com/user-attachments/assets/ca459090-3048-4ba0-a15f-a2004e6f1fb1" alt="Apache Config">
        </div>
        <div class="img-wrap">
            <img src="https://github.com/user-attachments/assets/35e18fd2-304f-4c8f-90c5-82ccefa8747b" alt="Softlink">
        </div>

        <p>Uso de repositorios externos para paquetes específicos como <em>antigravity</em>:</p>
        <div class="img-wrap">
            <img src="https://github.com/user-attachments/assets/5e8a9ae7-c40e-48ef-8fce-4544735c240f" alt="Antigravity">
        </div>
    </div>

    <div class="content-section">
        <h2>Repositorio nuevo para Google Chrome</h2>

        <p>Para descargar Google Chrome desde un repositorio externo, añadimos la clave y configuramos el repositorio oficial en el sistema.</p>

        <div class="code-block">sudo mkdir -p /etc/apt/keyrings
wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | sudo gpg --dearmor -o /etc/apt/keyrings/google-chrome.gpg</div>

        <p>Después añadimos el repositorio de Chrome al archivo de fuentes de APT:</p>

        <div class="code-block">echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/google-chrome.gpg] http://dl.google.com/linux/chrome/deb/ stable main" | sudo tee /etc/apt/sources.list.d/google-chrome.list</div>

        <p>Actualizamos la lista de paquetes e instalamos Chrome:</p>

        <div class="code-block">sudo apt update
sudo apt install google-chrome-stable</div>

        <p>En el servidor con <code>apt-mirror</code>, este repositorio permite descargar los paquetes de Chrome para servirlos desde la máquina configurada como repositorio local.</p>

        <div class="code-block">deb http://dl.google.com/linux/chrome/deb/ stable main
clean http://dl.google.com/linux/chrome/deb/</div>
    </div>

    <div class="content-section">
        <h2 class="sub">Configurar el cliente</h2>

        <p>Abrimos el archivo de repositorios del cliente:</p>
        <div class="code-block">nano /etc/apt/sources.list</div>
        <p><strong>Sources.list del cliente:</strong></p>
        <img src="https://github.com/user-attachments/assets/sp5-sources-list-client" alt="Sources.list cliente" class="course-image"/>

        <p>Como Google Chrome requiere firma, importamos su clave pública:</p>
        <p><strong>Error de firma (antes de importar la clave):</strong></p>
        <img src="https://github.com/user-attachments/assets/sp5-error-gpg-no-pubkey" alt="Error GPG NO_PUBKEY al hacer apt update" class="course-image"/>

        <p>Después ejecutamos <code>apt update</code> para comprobar que el cliente obtiene paquetes desde el servidor local:</p>
        <img src="https://github.com/user-attachments/assets/sp5-apt-update-client" alt="Apt update cliente desde repositorio local" class="course-image"/>

        <p>Instalamos el paquete desde el servidor:</p>
        <div class="code-block">apt install google-chrome-stable</div>
        <img src="https://github.com/user-attachments/assets/sp5-instalacion-chrome" alt="Instalación de Google Chrome desde el servidor local" class="course-image"/>

        <p>Y, como se ve en la captura, Google Chrome queda instalado correctamente:</p>
        <img src="https://github.com/user-attachments/assets/sp5-chrome-instalado" alt="Google Chrome instalado en el cliente" class="course-image"/>
    </div>


    <div class="content-section">
        <h2>Directorio de almacenamiento de registros</h2>
        <p>Listado del contenido de <code>/var/log</code>:</p>
        
        <div class="code-block">root@cliente:/var/log# ls
alternatives.log  btmp            kern.log          syslog
auth.log          dist-upgrade    lastlog           syslog.1
apt               dpkg.log        journal           unattended-upgrades
boot.log          faillog         private           wtmp
bootstrap.log     fontconfig.conf installer         vboxadd-setup.log</div>
    </div>

    <div class="content-section">
        <h2>Conceptos fundamentales de logging</h2>

        <p>Para gestionar los logs, Linux utiliza dos conceptos principales para clasificar la información:</p>

        <p><strong>Facility:</strong> define el origen o tipo de programa que genera el mensaje, por ejemplo <code>auth</code>, <code>cron</code>, <code>kern</code> o <code>mail</code>. El asterisco <code>*</code> indica todas las fuentes.</p>

        <p><strong>Priority:</strong> define la gravedad del mensaje, por ejemplo <code>debug</code>, <code>info</code>, <code>notice</code>, <code>warning</code>, <code>err</code>, <code>crit</code>, <code>alert</code> o <code>emerg</code>.</p>

        <p>Con punto <code>.</code> indicamos ese nivel y todos los superiores. Con igual <code>.=</code> indicamos solo ese nivel exacto.</p>

        <h2>La herramienta logger</h2>

        <p>El comando <code>logger</code> permite añadir entradas manuales al log del sistema desde terminal.</p>

        <div class="code-block">logger -i -s -p mail.err "Mensaje de error"</div>

        <p>La opción <code>-i</code> añade el PID del proceso, <code>-s</code> muestra el mensaje también por stderr y <code>-p</code> especifica la facility y la priority.</p>
    </div>

    <div class="content-section">
        <h2>Configuración de rsyslog</h2>

        <p>En Ubuntu/Debian, muchas reglas por defecto están en <code>/etc/rsyslog.d/50-default.conf</code>.</p>

        <p>Para monitorizar cambios en directo se puede usar:</p>

        <div class="code-block">tail -f /var/log/syslog</div>

        <p>Ejemplos de pruebas con <code>logger</code>:</p>

        <div class="code-block">logger -i -s -p kern.notice "Prueba"
logger -i -s -p mail.notice "Prueba"</div>

        <p>También se puede crear un log personalizado para registrar mensajes críticos:</p>

        <div class="code-block">*.crit    -/var/log/ivan.log</div>
    </div>

    <div class="content-section">
        <h2>journalctl</h2>

        <p>Además de logs en texto, <code>systemd</code> usa un journal binario consultable con <code>journalctl</code>.</p>

        <div class="code-block">journalctl --facility=mail</div>
    </div>

    <div class="content-section">
        <h2>Rendimiento y monitorización</h2>

        <p>Para observar el rendimiento del sistema usamos el Monitor del sistema de Ubuntu. Permite revisar procesos activos, consumo de recursos y estado de sistemas de archivos.</p>

        <p>La pestaña de procesos muestra procesos en ejecución con datos como PID, usuario, CPU y memoria.</p>

        <p>La pestaña de recursos muestra gráficas en tiempo real de CPU, RAM, swap y red.</p>

        <p>La pestaña de sistemas de archivos muestra discos y particiones con espacio usado y disponible.</p>
    </div>
</main>

