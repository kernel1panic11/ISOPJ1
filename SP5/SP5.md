---
layout: default
title: "Sprint 5. Logs y monitorización"
---

<link rel="stylesheet" href="{{ '/assets/css/style.css' | relative_url }}">

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
}

.contenedor-principal {
    max-width: 960px;
    margin: 0 auto;
    padding: 20px;
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

<main class="contenedor-principal">

    <div class="content-section">

        <h1>Sprint 5. Logs y monitorización</h1>

        <h2>Servidor de actualizaciones Ubuntu (CDN)</h2>

        <p>
        Empezamos instalando el <code>apache2</code><br>
        img/1
        </p>

        <div class="img-wrap">
            <img class="sp5-img" src="img/1.png" alt="Apache instalado">
        </div>

        <p>
        Después instalamos el <code>apt-mirror</code><br>
        img/2
        </p>

        <div class="img-wrap">
            <img class="sp5-img" src="img/2.png" alt="Instalación apt-mirror">
        </div>

        <p>
        En este archivo ponemos los repositorios que el servidor debe descargar.<br>
        img/3
        </p>

        <div class="img-wrap">
            <img class="sp5-img" src="img/3.png" alt="Configuración mirror.list">
        </div>

        <p>
        Descomentamos las repos y dejamos solo la de Chrome para ganar tiempo.<br>
        img/4
        </p>

        <div class="img-wrap">
            <img class="sp5-img" src="img/4.png" alt="Repositorios activos">
        </div>

        <p>
        Ejecutamos <code>apt-mirror</code> para descargar los paquetes del repositorio.<br>
        img/5
        </p>

        <div class="img-wrap">
            <img class="sp5-img" src="img/5.png" alt="Ejecución apt-mirror">
        </div>

        <p>
        Configuramos Apache.<br>
        img/6
        </p>

        <div class="img-wrap">
            <img class="sp5-img" src="img/6.png" alt="Configuración Apache">
        </div>

        <p>
        Creamos el <strong>softlink</strong>.<br>
        img/7
        </p>

        <div class="img-wrap">
            <img class="sp5-img" src="img/7.png" alt="Softlink creado">
        </div>

        <p>
        Uso de repositorios externos para paquetes específicos como <em>antigravity</em>.<br>
        img/8
        </p>

        <div class="img-wrap">
            <img class="sp5-img" src="img/8.png" alt="Repositorio antigravity">
        </div>

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
        Error de firma antes de importar la clave.<br>
        img/10
        </p>

        <div class="img-wrap">
            <img class="sp5-img" src="img/10.png" alt="Error GPG">
        </div>

        <p>
        Ejecutamos <code>apt update</code> para comprobar que el cliente obtiene paquetes desde el servidor local.<br>
        img/11
        </p>

        <div class="img-wrap">
            <img class="sp5-img" src="img/11.png" alt="Apt update cliente">
        </div>

        <p>
        Instalamos Google Chrome desde el servidor local.
        </p>

        <div class="code-block">apt install google-chrome-stable</div>

        <p>
        Instalación de Google Chrome desde el servidor local.<br>
        img/12
        </p>

        <div class="img-wrap">
            <img class="sp5-img" src="img/12.png" alt="Instalación Google Chrome">
        </div>

        <p>
        Como se observa en la captura, Google Chrome queda instalado correctamente en el cliente.
        </p>

        <div class="img-wrap">
            <img class="sp5-img" src="https://github.com/user-attachments/assets/b496a703-459b-40d3-9ee3-a0b03250142f" alt="Google Chrome instalado">
        </div>

    </div>

</main>
