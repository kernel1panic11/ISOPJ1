<div class="content-section">
    <h2>Configurar el cliente</h2>

    <p>
    Abrimos el archivo de repositorios del cliente.
    </p>

    <div class="code-block">nano /etc/apt/sources.list</div>

    <p>
    Sources.list del cliente<br>
    img/9
    </p>

    <div class="img-wrap">
        <img class="sp5-img" src="img/9.png" alt="Sources.list cliente">
    </div>

    <p>
    Como Google Chrome requiere firma, importamos su clave pública.
    </p>

    <p>
    Error de firma antes de importar la clave<br>
    img/10
    </p>

    <div class="img-wrap">
        <img class="sp5-img" src="img/10.png" alt="Error de firma GPG antes de importar la clave de Google Chrome">
    </div>

    <p>
    Después ejecutamos <code>apt update</code> para comprobar que el cliente obtiene los paquetes desde el servidor local.<br>
    img/11
    </p>

    <div class="img-wrap">
        <img class="sp5-img" src="img/11.png" alt="Apt update del cliente desde el repositorio local">
    </div>

    <p>
    Instalamos Google Chrome desde el servidor local.
    </p>

    <div class="code-block">apt install google-chrome-stable</div>

    <p>
    Instalación de Google Chrome desde el servidor local<br>
    img/12
    </p>

    <div class="img-wrap">
        <img class="sp5-img" src="img/12.png" alt="Instalación de Google Chrome desde el servidor local">
    </div>

    <p>
    Como se observa en la captura, Google Chrome queda instalado correctamente en el cliente.
    </p>

    <div class="img-wrap">
        <img class="sp5-img" src="https://github.com/user-attachments/assets/b496a703-459b-40d3-9ee3-a0b03250142f" alt="Google Chrome instalado correctamente">
    </div>
</div>
