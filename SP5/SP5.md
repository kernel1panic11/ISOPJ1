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

<h2>Conceptos fundamentales de logging</h2>

<p>
Para gestionar los logs, Linux utiliza dos conceptos principales para clasificar la información.
</p>

<p>
<strong>Facility:</strong> define el origen o tipo de programa que genera el mensaje.
</p>

<p>
<strong>Priority:</strong> define la gravedad del mensaje.
</p>

<p>
Con punto <code>.</code> indicamos ese nivel y todos los superiores.
Con <code>.=</code> indicamos únicamente ese nivel exacto.
</p>

<h2>La herramienta logger</h2>

<p>
El comando <code>logger</code> permite añadir entradas manuales al log del sistema desde terminal.
</p>

<div class="code-block">logger -i -s -p mail.err "Mensaje de error"</div>

<p>
La opción <code>-i</code> añade el PID del proceso,
<code>-s</code> muestra el mensaje también por stderr y
<code>-p</code> especifica la facility y la priority.
</p>

</div>

<div class="content-section">

<h2>Rotación de logs con logrotate</h2>

<p>
Acceso al directorio de configuración.<br>
img/15
</p>

<div class="img-wrap">
    <img class="sp5-img" src="img/15.png" alt="Directorio logrotate">
</div>

<p>
Configuración de rsyslog para logrotate.<br>
img/16
</p>

<div class="img-wrap">
    <img class="sp5-img" src="img/16.png" alt="Configuración rsyslog logrotate">
</div>

</div>

<div class="content-section">

<h2>Configuración de rsyslog</h2>

<p>
Archivo principal de configuración.<br>
img/17
</p>

<div class="img-wrap">
    <img class="sp5-img" src="img/17.png" alt="Configuración rsyslog">
</div>

<p>
Prueba con <code>kern.notice</code>.<br>
img/18
</p>

<div class="img-wrap">
    <img class="sp5-img" src="img/18.png" alt="Prueba kern.notice">
</div>

<p>
Prueba con <code>mail.notice</code>.<br>
img/19
</p>

<div class="img-wrap">
    <img class="sp5-img" src="img/19.png" alt="Prueba mail.notice">
</div>

<p>
Filtrado de niveles específicos.<br>
img/20
</p>

<div class="img-wrap">
    <img class="sp5-img" src="img/20.png" alt="Filtrado de niveles">
</div>

<p>
Configuración de logs personalizados.<br>
img/21
</p>

<div class="img-wrap">
    <img class="sp5-img" src="img/21.png" alt="Logs personalizados">
</div>

</div>

<div class="content-section">

<h2>journalctl</h2>

<p>
Los sistemas modernos utilizan <code>journalctl</code> para consultar logs binarios gestionados por systemd.
</p>

<div class="code-block">journalctl --facility=mail</div>

<p>
Consulta de logs mediante journalctl.<br>
img/22
</p>

<div class="img-wrap">
    <img class="sp5-img" src="img/22.png" alt="journalctl">
</div>

</div>

<div class="content-section">

<h2>Centralización de logs con rsyslog</h2>

<p>
Configuración IP de las máquinas.<br>
img/23
</p>

<div class="img-wrap">
    <img class="sp5-img" src="img/23.png" alt="Configuración IP">
</div>

<p>
Comprobación de IPs con <code>ip a</code>.<br>
img/24
</p>

<div class="img-wrap">
    <img class="sp5-img" src="img/24.png" alt="ip a">
</div>

<p>
Instalación de rsyslog.<br>
img/25
</p>

<div class="img-wrap">
    <img class="sp5-img" src="img/25.png" alt="Instalación rsyslog">
</div>

<p>
Activación de recepción UDP en el servidor.<br>
img/26
</p>

<div class="img-wrap">
    <img class="sp5-img" src="img/26.png" alt="UDP rsyslog">
</div>

<p>
Reinicio del servicio rsyslog.<br>
img/27
</p>

<div class="img-wrap">
    <img class="sp5-img" src="img/27.png" alt="Restart rsyslog">
</div>

<p>
Configuración del cliente para enviar logs al servidor.<br>
img/28
</p>

<div class="img-wrap">
    <img class="sp5-img" src="img/28.png" alt="Cliente rsyslog">
</div>

<p>
Recepción correcta de logs.<br>
img/29
</p>

<div class="img-wrap">
    <img class="sp5-img" src="img/29.png" alt="Recepción logs">
</div>

</div>

<div class="content-section">

<h2>Servidor de actualizaciones Ubuntu (CDN)</h2>

<p>
Instalación de Apache2.<br>
img/1
</p>

<div class="img-wrap">
    <img class="sp5-img" src="img/1.png" alt="Apache">
</div>

<p>
Instalación de apt-mirror.<br>
img/2
</p>

<div class="img-wrap">
    <img class="sp5-img" src="img/2.png" alt="apt-mirror">
</div>

<p>
Configuración de mirror.list.<br>
img/3
</p>

<div class="img-wrap">
    <img class="sp5-img" src="img/3.png" alt="mirror.list">
</div>

<p>
Repositorios activos.<br>
img/4
</p>

<div class="img-wrap">
    <img class="sp5-img" src="img/4.png" alt="Repositorios">
</div>

<p>
Ejecución de apt-mirror.<br>
img/5
</p>

<div class="img-wrap">
    <img class="sp5-img" src="img/5.png" alt="apt-mirror ejecución">
</div>

<p>
Configuración Apache.<br>
img/6
</p>

<div class="img-wrap">
    <img class="sp5-img" src="img/6.png" alt="Apache config">
</div>

<p>
Softlink creado.<br>
img/7
</p>

<div class="img-wrap">
    <img class="sp5-img" src="img/7.png" alt="Softlink">
</div>

<p>
Repositorio externo antigravity.<br>
img/8
</p>

<div class="img-wrap">
    <img class="sp5-img" src="img/8.png" alt="Antigravity">
</div>

</div>

<div class="content-section">

<h2>Configurar el cliente</h2>

<div class="code-block">nano /etc/apt/sources.list</div>

<p>
Sources.list del cliente.<br>
img/9
</p>

<div class="img-wrap">
    <img class="sp5-img" src="img/9.png" alt="sources.list">
</div>

<p>
Error GPG antes de importar la clave.<br>
img/10
</p>

<div class="img-wrap">
    <img class="sp5-img" src="img/10.png" alt="Error GPG">
</div>

<p>
Instalación de rsyslog en el cliente.<br>
img/11
</p>

<div class="img-wrap">
    <img class="sp5-img" src="img/11.png" alt="Instalación rsyslog">
</div>

<div class="code-block">apt install google-chrome-stable</div>

<p>
Instalación de Google Chrome desde el servidor local.<br>
img/12
</p>

<div class="img-wrap">
    <img class="sp5-img" src="img/12.png" alt="Instalación Google Chrome">
</div>

<p>
Google Chrome instalado correctamente.
</p>

<div class="img-wrap">
    <img class="sp5-img" src="https://github.com/user-attachments/assets/b496a703-459b-40d3-9ee3-a0b03250142f" alt="Google Chrome instalado">
</div>

</div>

</main>
