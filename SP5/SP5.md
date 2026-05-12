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
        
        <p>
        Empezamos instalando el <code>apache2</code><br>
        SP5/img/1
        </p>

        <div class="img-wrap">
            <img src="{{ "/SP5/img/1.png" | relative_url }}" alt="Apache">
        </div>

        <p>
        Después instalamos el <code>apt-mirror</code><br>
        SP5/img/2
        </p>

        <div class="img-wrap">
            <img src="{{ "/SP5/img/2.png" | relative_url }}" alt="Apt-mirror">
        </div>

        <p>
        En este archivo ponemos los repositorios que el servidor debe descargar<br>
        SP5/img/3
        </p>

        <div class="img-wrap">
            <img src="{{ "/SP5/img/3.png" | relative_url }}" alt="Configuración">
        </div>

        <p>
        Descomentamos las repos y dejamos solo la de Chrome para ganar tiempo<br>
        SP5/img/4
        </p>

        <div class="img-wrap">
            <img src="{{ "/SP5/img/4.png" | relative_url }}" alt="Repositorios">
        </div>

        <p>
        Ejecutamos <code>apt-mirror</code> para descargar los paquetes del repositorio<br>
        SP5/img/5
        </p>

        <div class="img-wrap">
            <img src="{{ "/SP5/img/5.png" | relative_url }}" alt="Ejecución">
        </div>

        <p>
        Configuramos Apache y creamos el <strong>softlink</strong><br>
        SP5/img/6
        </p>

        <div class="img-wrap">
            <img src="{{ "/SP5/img/6.png" | relative_url }}" alt="Apache Config">
        </div>

        <p>
        Creación del softlink<br>
        SP5/img/7
        </p>

        <div class="img-wrap">
            <img src="{{ "/SP5/img/7.png" | relative_url }}" alt="Softlink">
        </div>

        <p>
        Uso de repositorios externos para paquetes específicos como <em>antigravity</em><br>
        SP5/img/8
        </p>

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

        <p>
        Sources.list del cliente<br>
        SP5/img/9
        </p>

        <div class="img-wrap">
            <img src="{{ "/SP5/img/9.png" | relative_url }}" alt="Sources.list cliente">
        </div>

        <p>Como Google Chrome requiere firma, importamos su clave pública:</p>

        <p>
        Error de firma antes de importar la clave<br>
        SP5/img/10
        </p>

        <div class="img-wrap">
            <img src="{{ "/SP5/img/10.png" | relative_url }}" alt="Error GPG">
        </div>

        <p>
        Ejecutamos <code>apt update</code> para comprobar que el cliente obtiene paquetes desde el servidor local<br>
        SP5/img/11
        </p>

        <div class="img-wrap">
            <img src="{{ "/SP5/img/11.png" | relative_url }}" alt="Apt update">
        </div>

        <p>Instalamos el paquete desde el servidor:</p>

        <div class="code-block">apt install google-chrome-stable</div>

        <p>
        Instalación de Google Chrome desde el servidor local<br>
        SP5/img/12
        </p>

        <div class="img-wrap">
            <img src="{{ "/SP5/img/12.png" | relative_url }}" alt="Chrome">
        </div>

        <p>
        Google Chrome instalado correctamente<br>
        SP5/img/13
        </p>

        <div class="img-wrap">
            <img src="{{ "/SP5/img/13.png" | relative_url }}" alt="Chrome instalado">
        </div>
    </div>
</main>
