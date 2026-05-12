<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sprint 5. Logs y monitorización</title>

    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">

    <style>
        @import url('https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Rajdhani:wght@400;600;700&family=Exo+2:wght@300;400;600&display=swap');

        :root {
            --bg: #0a0b10;
            --surface: rgba(14, 16, 26, 0.85);
            --border: rgba(0, 255, 255, 0.18);
            --accent: #00ffff;
            --accent2: #7b2fff;
            --text: #e8eaf0;
            --code-bg: #0d1117;
            --radius: 10px;
            --shadow-neon: 0 0 18px rgba(0,255,255,0.25);
        }

        body {
            background-color: var(--bg);
            color: var(--text);
            font-family: 'Exo 2', sans-serif;
            line-height: 1.75;
            margin: 0;
            padding: 20px;
        }

        .contenedor-principal {
            max-width: 960px;
            margin: 0 auto;
        }

        .content-section {
            background: var(--surface);
            border: 1px solid var(--border);
            border-radius: var(--radius);
            padding: 2.5rem;
            box-shadow: var(--shadow-neon);
            margin-bottom: 30px;
        }

        h1, h2 {
            font-family: 'Rajdhani', sans-serif;
            text-transform: uppercase;
            color: var(--accent);
        }

        .img-wrap {
            margin: 20px 0;
            text-align: center;
        }

        .sp5-img {
            max-width: 100%;
            border-radius: 8px;
            border: 1px solid var(--border);
        }

        .code-block {
            background: var(--code-bg);
            border-left: 4px solid var(--accent2);
            padding: 15px;
            margin: 20px 0;
            font-family: 'Share Tech Mono', monospace;
            color: #79c0ff;
            overflow-x: auto;
            white-space: pre;
        }
    </style>
</head>

<body>

<main class="contenedor-principal">

    <div class="content-section">
        <h1>Sprint 5. Logs y monitorización</h1>

        <h2>Servidor de actualizaciones Ubuntu (CDN)</h2>

        <p>
        Empezamos instalando el <code>apache2</code><br>
        img/1
        </p>

        <div class="img-wrap">
            <img class="sp5-img" src="img/1.png" alt="Apache">
        </div>

        <p>
        Después instalamos el <code>apt-mirror</code><br>
        img/2
        </p>

        <div class="img-wrap">
            <img class="sp5-img" src="img/2.png" alt="Apt-mirror">
        </div>

        <p>
        En este archivo ponemos los repositorios que el servidor debe descargar<br>
        img/3
        </p>

        <div class="img-wrap">
            <img class="sp5-img" src="img/3.png" alt="Configuración CDN">
        </div>

        <p>
        Descomentamos las repos y dejamos solo la de Chrome para ganar tiempo<br>
        img/4
        </p>

        <div class="img-wrap">
            <img class="sp5-img" src="img/4.png" alt="Repositorios">
        </div>

        <p>
        Ejecutamos <code>apt-mirror</code> para descargar los paquetes del repositorio<br>
        img/5
        </p>

        <div class="img-wrap">
            <img class="sp5-img" src="img/5.png" alt="Ejecución apt-mirror">
        </div>

        <p>
        Configuramos Apache<br>
        img/6
        </p>

        <div class="img-wrap">
            <img class="sp5-img" src="img/6.png" alt="Configuración Apache">
        </div>

        <p>
        Creamos el <strong>softlink</strong><br>
        img/7
        </p>

        <div class="img-wrap">
            <img class="sp5-img" src="img/7.png" alt="Softlink">
        </div>

        <p>
        Uso de repositorios externos para paquetes específicos como <em>antigravity</em><br>
        img/8
        </p>

        <div class="img-wrap">
            <img class="sp5-img" src="img/8.png" alt="Antigravity">
        </div>
    </div>

    <div class="content-section">
        <h2>Repositorio nuevo para Google Chrome</h2>

        <p>
        Para descargar Google Chrome desde un repositorio externo, añadimos la clave y configuramos el repositorio oficial en el sistema.
        </p>

        <div class="code-block">sudo mkdir -p /etc/apt/keyrings
wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | sudo gpg --dearmor -o /etc/apt/keyrings/google-chrome.gpg</div>

        <p>
        Después añadimos el repositorio de Chrome al archivo de fuentes de APT.
        </p>

        <div class="code-block">echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/google-chrome.gpg] http://dl.google.com/linux/chrome/deb/ stable main" | sudo tee /etc/apt/sources.list.d/google-chrome.list</div>

        <p>
        Actualizamos la lista de paquetes e instalamos Chrome.
        </p>

        <div class="code-block">sudo apt update
sudo apt install google-chrome-stable</div>

        <p>
        En el servidor con <code>apt-mirror</code>, este repositorio permite descargar los paquetes de Chrome para servirlos desde la máquina configurada como repositorio local.
        </p>

        <div class="code-block">deb http://dl.google.com/linux/chrome/deb/ stable main
clean http://dl.google.com/linux/chrome/deb/</div>
    </div>

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
            <img class="sp5-img" src="img/10.png" alt="Error GPG">
        </div>

        <p>
        Después ejecutamos <code>apt update</code> para comprobar que el cliente obtiene paquetes desde el servidor local<br>
        img/11
        </p>

        <div class="img-wrap">
            <img class="sp5-img" src="img/11.png" alt="Apt update cliente">
        </div>

        <p>
        Instalamos el paquete desde el servidor.
        </p>

        <div class="code-block">apt install google-chrome-stable</div>

        <p>
        Instalación de Google Chrome desde el servidor local<br>
        img/12
        </p>

        <div class="img-wrap">
            <img class="sp5-img" src="img/12.png" alt="Instalación Google Chrome">
        </div>

        <p>
        Google Chrome queda instalado correctamente<br>
        img/13
        </p>

        <div class="img-wrap">
            <img class="sp5-img" src="img/13.png" alt="Google Chrome instalado">
        </div>
    </div>

</main>

</body>
</html>
