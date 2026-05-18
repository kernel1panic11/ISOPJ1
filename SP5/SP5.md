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
            <img src="{{ "/SP5/img/1.png" | relative_url }}" alt="Apache">
        </div>

        <p>Después instalamos el <code>apt-mirror</code>:</p>
        <div class="img-wrap">
            <img src="{{ "/SP5/img/2.png" | relative_url }}" alt="Apt-mirror">
        </div>

        <p>En este archivo ponemos los repositorios que el servidor debe descargar:</p>
        <div class="img-wrap">
            <img src="{{ "/SP5/img/3.png" | relative_url }}" alt="Configuración">
        </div>

        <p>Descomentamos las repos y dejamos solo la de Chrome para ganar tiempo:</p>
        <div class="img-wrap">
            <img src="{{ "/SP5/img/4.png" | relative_url }}" alt="Repositorios">
        </div>

        <p>Ejecutamos <code>apt-mirror</code> para descargar los paquetes del repositorio:</p>
        <div class="img-wrap">
            <img src="{{ "/SP5/img/5.png" | relative_url }}" alt="Ejecución">
        </div>

        <p>Configuramos Apache y creamos el <strong>softlink</strong>:</p>
        <div class="img-wrap">
            <img src="{{ "/SP5/img/6.png" | relative_url }}" alt="Apache Config">
        </div>
        <div class="img-wrap">
            <img src="{{ "/SP5/img/7.png" | relative_url }}" alt="Softlink">
        </div>

        <p>Uso de repositorios externos para paquetes específicos como <em>antigravity</em>:</p>
        <div class="img-wrap">
            <img src="{{ "/SP5/img/8.png" | relative_url }}" alt="Antigravity">
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
        <img src="{{ "/SP5/img/9.png" | relative_url }}" alt="Sources.list cliente" class="course-image"/>

        <p>Como Google Chrome requiere firma, importamos su clave pública:</p>
        <p><strong>Error de firma (antes de importar la clave):</strong></p>
        <img src="{{ "/SP5/img/10.png" | relative_url }}" alt="Error GPG NO_PUBKEY al hacer apt update" class="course-image"/>

        <p>Después ejecutamos <code>apt update</code> para comprobar que el cliente obtiene paquetes desde el servidor local:</p>
        <img src="{{ "/SP5/img/11.png" | relative_url }}" alt="Apt update cliente desde repositorio local" class="course-image"/>

        <p>Instalamos el paquete desde el servidor:</p>
        <div class="code-block">apt install google-chrome-stable</div>
        <img src="{{ "/SP5/img/12.png" | relative_url }}" alt="Instalación de Google Chrome desde el servidor local" class="course-image"/>

        <p>Y, como se ve en la captura, Google Chrome queda instalado correctamente:</p>
        <img src="{{ "/SP5/img/13.png" | relative_url }}" alt="Google Chrome instalado en el cliente" class="course-image"/>
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

    <div class="content-section">
        <h2>Evidencias adicionales del sprint (capturas 14 a 35)</h2>
        <p>Estas capturas completan toda la evidencia visual almacenada en <code>SP5/img</code> y se mantienen dentro del mismo contenedor del documento.</p>

        <h3>Captura 14</h3>
        <div class="img-wrap">
            <img src="{{ "/SP5/img/14.png" | relative_url }}" alt="Captura 14 del Sprint 5">
        </div>

        <h3>Captura 15</h3>
        <div class="img-wrap">
            <img src="{{ "/SP5/img/15.png" | relative_url }}" alt="Captura 15 del Sprint 5">
        </div>

        <h3>Captura 16</h3>
        <div class="img-wrap">
            <img src="{{ "/SP5/img/16.png" | relative_url }}" alt="Captura 16 del Sprint 5">
        </div>

        <h3>Captura 17</h3>
        <div class="img-wrap">
            <img src="{{ "/SP5/img/17.png" | relative_url }}" alt="Captura 17 del Sprint 5">
        </div>

        <h3>Captura 18</h3>
        <div class="img-wrap">
            <img src="{{ "/SP5/img/18.png" | relative_url }}" alt="Captura 18 del Sprint 5">
        </div>

        <h3>Captura 19</h3>
        <div class="img-wrap">
            <img src="{{ "/SP5/img/19.png" | relative_url }}" alt="Captura 19 del Sprint 5">
        </div>

        <h3>Captura 20</h3>
        <div class="img-wrap">
            <img src="{{ "/SP5/img/20.png" | relative_url }}" alt="Captura 20 del Sprint 5">
        </div>

        <h3>Captura 21</h3>
        <div class="img-wrap">
            <img src="{{ "/SP5/img/21.png" | relative_url }}" alt="Captura 21 del Sprint 5">
        </div>

        <h3>Captura 22</h3>
        <div class="img-wrap">
            <img src="{{ "/SP5/img/22.png" | relative_url }}" alt="Captura 22 del Sprint 5">
        </div>

        <h3>Captura 23</h3>
        <div class="img-wrap">
            <img src="{{ "/SP5/img/23.png" | relative_url }}" alt="Captura 23 del Sprint 5">
        </div>

        <h3>Captura 24</h3>
        <div class="img-wrap">
            <img src="{{ "/SP5/img/24.png" | relative_url }}" alt="Captura 24 del Sprint 5">
        </div>

        <h3>Captura 25</h3>
        <div class="img-wrap">
            <img src="{{ "/SP5/img/25.png" | relative_url }}" alt="Captura 25 del Sprint 5">
        </div>

        <h3>Captura 26</h3>
        <div class="img-wrap">
            <img src="{{ "/SP5/img/26.png" | relative_url }}" alt="Captura 26 del Sprint 5">
        </div>

        <h3>Captura 27</h3>
        <div class="img-wrap">
            <img src="{{ "/SP5/img/27.png" | relative_url }}" alt="Captura 27 del Sprint 5">
        </div>

        <h3>Captura 28</h3>
        <div class="img-wrap">
            <img src="{{ "/SP5/img/28.png" | relative_url }}" alt="Captura 28 del Sprint 5">
        </div>

        <h3>Captura 29</h3>
        <div class="img-wrap">
            <img src="{{ "/SP5/img/29.png" | relative_url }}" alt="Captura 29 del Sprint 5">
        </div>

        <h3>Captura 30</h3>
        <div class="img-wrap">
            <img src="{{ "/SP5/img/30.png" | relative_url }}" alt="Captura 30 del Sprint 5">
        </div>

        <h3>Captura 31</h3>
        <div class="img-wrap">
            <img src="{{ "/SP5/img/31.png" | relative_url }}" alt="Captura 31 del Sprint 5">
        </div>

        <h3>Captura 32</h3>
        <div class="img-wrap">
            <img src="{{ "/SP5/img/32.png" | relative_url }}" alt="Captura 32 del Sprint 5">
        </div>

        <h3>Captura 33</h3>
        <div class="img-wrap">
            <img src="{{ "/SP5/img/33.png" | relative_url }}" alt="Captura 33 del Sprint 5">
        </div>

        <h3>Captura 34</h3>
        <div class="img-wrap">
            <img src="{{ "/SP5/img/34.png" | relative_url }}" alt="Captura 34 del Sprint 5">
        </div>

        <h3>Captura 35</h3>
        <div class="img-wrap">
            <img src="{{ "/SP5/img/35.png" | relative_url }}" alt="Captura 35 del Sprint 5">
        </div>
    </div>
</main>
