<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Spring 5. Logs y monitorización</title>
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
            --code-fg: #58a6ff;
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

        .img-wrap img {
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
        <h1>Servidor de actualizaciones Ubuntu (CDN)</h1>
        
        <p>Empezamos instalando el <code>apache2</code>:</p>
        <div class="img-wrap">
            <img src="https://github.com/user-attachments/assets/a4b43d1e-6732-43f2-9155-83a3a02b3c9c" alt="Apache">
        </div>

        <p>Después instalamos el <code>apt-mirror</code>:</p>
        <div class="img-wrap">
            <img src="https://github.com/user-attachments/assets/55e4665d-e127-4ae7-a1c9-d148b55c5c1f" alt="Apt-mirror">
        </div>

        <p>En este archivo ponemos los repositorios que el servidor debe descargar (CDN):</p>
        <div class="img-wrap">
            <img src="https://github.com/user-attachments/assets/0302cb80-cd2b-4e03-b290-1eb47eac7ef9" alt="Configuración">
        </div>

        <p>Descomentamos las repos y dejamos solo la de Chrome para ganar tiempo:</p>
        <div class="img-wrap">
            <img src="https://github.com/user-attachments/assets/494e569b-4820-4c51-91bc-fdaf366c070e" alt="Repositorios">
        </div>

        <p>Ejecutamos <code>apt-mirror</code> para descargar la aplicación:</p>
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
        <h2>Directorio de almacenamiento de registros</h2>
        <p>Listado del contenido de <code>/var/log</code>:</p>
        
        <div class="code-block">
root@cliente:/var/log# ls
alternatives.log  btmp            kern.log          syslog
auth.log          dist-upgrade    lastlog           syslog.1
apt               dpkg.log        journal           unattended-upgrades
boot.log          faillog         private           wtmp
bootstrap.log     fontconfig.conf installer         vboxadd-setup.log
        </div>
    </div>
</main>

</body>
</html>
