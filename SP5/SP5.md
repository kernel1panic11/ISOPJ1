---
layout: default
title: "Sprint 5: Monitorización, Auditorías y Software Cliente/Servidor"
---

<main class="contenedor-principal">
  <h1 class="titulo">Sprint 5: Monitorización, Auditorías y Software Cliente/Servidor</h1>
  <div class="loading-bar"><div class="loading-progress"></div></div>

## 1. Conceptos fundamentales de logging

Para gestionar los logs, Linux utiliza dos conceptos principales para clasificar la información:

* **Facility:** define el origen o tipo de programa que genera el mensaje (por ejemplo: `auth`, `cron`, `kern`, `mail`). El asterisco (`*`) indica todas las fuentes.
* **Priority (nivel):** define la gravedad del mensaje (por ejemplo: `debug`, `info`, `notice`, `warning`, `err`, `crit`, `alert`, `emerg`).
* Con punto (`.`) indicamos ese nivel y todos los superiores.
* Con igual (`.=`) indicamos solo ese nivel exacto.

### La herramienta `logger`

El comando `logger` permite añadir entradas manuales al log del sistema desde terminal.

> **Ejemplo:** `logger -i -s -p mail.err "Mensaje de error"`
> * `-i`: añade el PID del proceso.
> * `-s`: muestra el mensaje también por stderr.
> * `-p`: especifica *facility* y *priority*.

---

## 2. Directorios y archivos importantes

La mayoría de logs del sistema se almacenan en `/var/log`. Aunque muchos servicios escriben en `syslog`, otros paquetes crean sus propias rutas.

<img width="1181" height="235" alt="image" src="https://github.com/user-attachments/assets/a4107def-0e00-4b8b-9cd2-d9e2b57926f1" />

Si visualizamos `syslog`, podemos ver actividad general del sistema en tiempo real.

<img width="1208" height="709" alt="image" src="https://github.com/user-attachments/assets/7b87c153-76c5-4a76-8bdd-89f41addc4ca" />

---

## 3. Rotación de logs (`logrotate`)

Para evitar que los logs llenen el disco, `logrotate`:

1. Comprime logs antiguos (`.gz`).
2. Los renombra (`syslog.1`, `syslog.2.gz`).
3. Elimina logs viejos según días o tamaño.

La configuración se encuentra en `/etc/logrotate.d/`.

<img width="1012" height="109" alt="image" src="https://github.com/user-attachments/assets/8470044a-eda1-4ee9-973c-e55b6c4faed9" />

---

## 4. Configuración de `rsyslog`

En Ubuntu/Debian, muchas reglas por defecto están en `/etc/rsyslog.d/50-default.conf`.

<img width="1233" height="683" alt="image" src="https://github.com/user-attachments/assets/cc484c0c-752c-4122-8982-aad728ef4138" />

### Pruebas de funcionamiento

Para monitorizar cambios en directo:
`tail -f /var/log/syslog`

1. `logger -i -s -p kern.notice "Prueba"`
2. `logger -i -s -p mail.notice "Prueba"`
3. Probar filtros con y sin `=` en `mail.err`.
4. Crear log personalizado, por ejemplo:
`*.crit -/var/log/pau.log`

---

## 5. `journalctl`

Además de logs en texto, `systemd` usa un journal binario consultable con `journalctl`.

Ejemplo:
`journalctl --facility=mail`

<img width="646" height="69" alt="image" src="https://github.com/user-attachments/assets/32039832-b9f5-4be2-a548-0a72b9e1c1c6" />

---

## TAREA 1: Rendimiento y monitorización

Para observar el rendimiento del sistema usamos el **Monitor del sistema de Ubuntu**. Permite revisar procesos activos, consumo de recursos y estado de sistemas de archivos.

### Pestaña 1: Procesos

Muestra procesos en ejecución con datos como **PID, usuario, CPU y memoria**.

<img width="699" height="590" alt="image" src="https://github.com/user-attachments/assets/f92f86ba-7f1e-4b39-9439-7c40bb3563bc" />

### Pestaña 2: Recursos

Muestra gráficas en tiempo real de **CPU, RAM, swap y red**.

<img width="676" height="504" alt="image" src="https://github.com/user-attachments/assets/a2730a83-b391-49a2-b416-14d550ec88b6" />

### Pestaña 3: Sistemas de archivos

Muestra discos/particiones con **espacio usado y disponible**.

<img width="657" height="308" alt="image" src="https://github.com/user-attachments/assets/edad65f4-baae-44fc-9216-5b729c62c817" />

---

# TAREA CONJUNTA: Centralización de logs con `rsyslog`

**Fecha:** 03/03/26  
**Componentes:** Valle (Grupo A), Pau (Grupo B)

Se configura una máquina como servidor de logs y otra como cliente para enviar registros por red.

## Paso 1: Preparación (ambos hosts)

* Configurar IPs y comprobar con `ip a`.
* Desactivar firewall temporalmente: `sudo ufw disable`.
* Verificar `rsyslog`: `sudo apt update && sudo apt install rsyslog -y`.

## Paso 2: Servidor

Editar:
`sudo nano /etc/rsyslog.conf`

Habilitar UDP:
```
module(load="imudp")
input(type="imudp" port="514")
```

Reiniciar:
`sudo systemctl restart rsyslog`

## Paso 3: Cliente

Editar:
`sudo nano /etc/rsyslog.d/50-default.conf`

Añadir:
`*.* @192.168.1.10:514`

Reiniciar:
`sudo systemctl restart rsyslog`

## Paso 4: Verificación

En servidor:
`tail -f /var/log/syslog`

En cliente:
`logger "prueba de envío de logs"`

---

# Servidor de actualizaciones

**Fecha:** 09/03/26

Documentación para montar servidor local de paquetes con `apt-mirror` + Apache y usarlo desde clientes.

## 1) Servidor

```
sudo su
apt update
apt install apache2
apt install apt-mirror
```

## 2) Configurar `apt-mirror`

Editar:
`nano /etc/apt/mirror.list`

Para la práctica, dejar solo repositorio de Google Chrome.

Ejecutar:
`apt-mirror`

## 3) Exponer con Apache

Crear softlink:
`ln -s /var/spool/apt-mirror/mirror/dl.google.com/linux/chrome/deb /var/www/html/`

## 4) Cliente

Editar repositorios:
`nano /etc/apt/sources.list`

Importar firma de Chrome, actualizar e instalar:

```
apt update
apt install google-chrome-stable
```

</main>

<style>
:root {
  --bg-image: url('{{ "/assetscss/pract22.gif" | relative_url }}');
}

main.contenedor-principal img {
  width: 100%;
  max-width: 1000px;
  display: block;
  margin: 1rem auto;
  border-radius: var(--radius);
  border: 1px solid var(--border);
  box-shadow: var(--shadow-neon);
}
</style>
