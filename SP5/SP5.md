---
layout: default
title: "Sprint 5: Monitoratge, Auditories i Programari Client/Servidor"
---

<main class="contenedor-principal">
  <h1 class="titulo">Sprint 5: Monitoratge, Auditories i Programari Client/Servidor</h1>
  <div class="loading-bar"><div class="loading-progress"></div></div>

## 1. Conceptes Fonamentals de Logging

Per gestionar els logs, Linux utilitza dos conceptes principals per classificar la informació:

* **Facility:** Defineix l'origen o el tipus de programa que genera el missatge (ex: `auth`, `cron`, `kern`, `mail`). L'asterisc (`*`) s'utilitza per indicar totes les fonts.
* **Priority (Nivell):** Defineix la gravetat del missatge (ex: `debug`, `info`, `notice`, `warning`, `err`, `crit`, `alert`, `emerg`).
* Si posem un punt (`.`), estem indicant aquest nivell i tots els superiors.
* Si posem un igual (`.=`), indiquem **només** aquell nivell específic.


### L'eina `logger`

La comanda `logger` permet afegir entrades al log del sistema manualment des de la terminal.

> **Exemple:** `logger -i -s -p mail.err "Missatge d'error"`
> * `-i`: Afegeix el PID del procés.
> * `-s`: Mostra el missatge també per la sortida d'error estàndard.
> * `-p`: Especifica la *facility* i la *prioritat*.
---

## 2. Directoris i Fitxers Importants

La majoria dels logs del sistema s'emmagatzemen a `/var/log`. Tot i que molts serveis escriuen al fitxer general `syslog`, alguns paquets instal·lats creen les seves pròpies carpetes per gestionar les seves dades de manera independent.

<img width="1181" height="235" alt="image" src="https://github.com/user-attachments/assets/a4107def-0e00-4b8b-9cd2-d9e2b57926f1" />

Si visualitzem el fitxer `syslog`, podem veure l'activitat general del sistema en temps real:

<img width="1208" height="709" alt="image" src="https://github.com/user-attachments/assets/7b87c153-76c5-4a76-8bdd-89f41addc4ca" />

---

## 3. Rotació de Logs (`logrotate`)

Per evitar que els fitxers de log omplin tot el disc dur, el sistema utilitza **logrotate**. Aquesta eina:

1. Comprimeix els logs antics (sovint en format `.gz`).
2. Els reanomena (ex: `syslog.1`, `syslog.2.gz`).
3. Elimina els fitxers més vells segons una configuració de dies o mida.

La configuració es troba a `/etc/logrotate.d/`.

<img width="1012" height="109" alt="image" src="https://github.com/user-attachments/assets/8470044a-eda1-4ee9-973c-e55b6c4faed9" />

Podem veure la configuració específica de `rsyslog` per entendre com gestiona els seus propis fitxers:

<img width="987" height="536" alt="image" src="https://github.com/user-attachments/assets/059936de-5357-460b-878b-4a96f5aabba8" />

---

## 4. Configuració de `rsyslog`

Antigament, tota la configuració es trobava a `/etc/rsyslog.conf`. Actualment, a les distribucions basades en Ubuntu/Debian, la configuració de les regles per defecte es troba a:
`/etc/rsyslog.d/50-default.conf`

<img width="1233" height="683" alt="image" src="https://github.com/user-attachments/assets/cc484c0c-752c-4122-8982-aad728ef4138" />

### Proves de funcionament

Per veure els canvis en directe mentre fem proves, executem en una terminal:
`tail -f /var/log/syslog`

####  1: Kern.notice

Executem: `logger -i -s -p kern.notice "Prova pau"`
El resultat a la terminal de monitoratge serà:

<img width="807" height="460" alt="image" src="https://github.com/user-attachments/assets/ff3595b7-112e-4d89-9393-5df407d9a5c4" />



#### Prova 2: Mail.notice

Podem provar amb diferents serveis com el de correu:


<img width="856" height="457" alt="image" src="https://github.com/user-attachments/assets/eff3f65b-138f-417e-a861-e4a99fad8712" />

<img width="917" height="333" alt="image" src="https://github.com/user-attachments/assets/5764edfe-4c00-490d-92f8-d2e4d4d261c4" />

#### Prova 3 i 4: Filtres de nivell

Si modifiquem el fitxer de configuració per filtrar nivells específics (per exemple, usant o traient l'igual `=` a `mail.err`), el comportament del log canviarà, registrant només aquest nivell o tots els superiors.

<img width="784" height="129" alt="image (10)" src="https://github.com/user-attachments/assets/8c430e9e-0c90-483f-b027-3e9fd4d0a64c" />
<img width="1231" height="409" alt="image" src="https://github.com/user-attachments/assets/1cbf2e38-065e-4ca2-8207-5c42abeebff9" />

#### Prova 5: Fitxers de log personalitzats

Podem redirigir logs a fitxers propis afegint una línia al fitxer `50-default.conf`. Per exemple, per enviar totes les alertes crítiques a un fitxer nou:
`*crit -/var/log/pau.log`

<img width="808" height="342" alt="image" src="https://github.com/user-attachments/assets/fe5fed93-f0e2-461a-8012-f54bd46821f8" />

---

## 5. El sistema `journalctl`

A més dels fitxers de text tradicionals, els sistemes moderns amb `systemd` utilitzen un log binari gestionat per `journalctl`. Això permet fer cerques molt més ràpides i filtrades.

Per exemple, per veure només els logs de correu:
`journalctl --facility=mail`

<img width="646" height="69" alt="image" src="https://github.com/user-attachments/assets/32039832-b9f5-4be2-a548-0a72b9e1c1c6" />

---

## TASCA 1: Rendiment

Per observar el rendiment del sistema utilitzem el **Monitor del sistema d’Ubuntu**, una eina gràfica que permet veure informació sobre els processos actius, el consum de recursos i l’estat dels sistemes de fitxers.

Aquesta eina és útil per supervisar el funcionament del sistema i detectar possibles problemes de rendiment.

---

### Pestanya 1: Processos

En aquesta pestanya podem veure **tots els processos que s’estan executant al sistema**.  
També mostra informació com el **PID, l’usuari, el percentatge de CPU i la memòria utilitzada** per cada procés.

<img width="699" height="590" alt="image" src="https://github.com/user-attachments/assets/f92f86ba-7f1e-4b39-9439-7c40bb3563bc" />

---

### Pestanya 2: Recursos

En aquesta pestanya es mostren **gràfiques del consum de recursos del sistema**, com l’ús de **CPU, memòria RAM, memòria swap i xarxa**.  
Això permet veure l’activitat del sistema en temps real.

<img width="676" height="504" alt="image" src="https://github.com/user-attachments/assets/a2730a83-b391-49a2-b416-14d550ec88b6" />

---

### Pestanya 3: Sistemes de fitxers

En aquesta secció es mostra informació sobre els **discos i particions del sistema**, indicant l’**espai utilitzat i l’espai disponible** a cada sistema de fitxers.

<img width="657" height="308" alt="image" src="https://github.com/user-attachments/assets/edad65f4-baae-44fc-9216-5b729c62c817" />

---  

# TASCA CONJUNTA: Configuració de Rsyslog per a Enviament de Logs entre Hosts

**Data:** 03/03/26

**Components:**

Valle (Grup A)  
Pau (Grup B)

En aquesta pràctica configurem un sistema de **centralització de logs** utilitzant `rsyslog` entre dues màquines Ubuntu dins la mateixa xarxa.
Imaginem que un profe vol visualitzar els logs dels alumnes. En aquesta pràctica simularem aquest escenari:
Un host actuarà com a **servidor de logs (Profe)** i l’altre com a **client (Alumne)** que enviarà els seus registres al servidor.

---

# Pas 1: Preparació de les màquines (ambdós hosts)

Primer de tot configurem la xarxa i preparem les màquines perquè es puguin comunicar correctament.

## Configuració de les IPs

Configurem les adreces IP a cada màquina, ja sigui des de la configuració de xarxa d’Ubuntu o utilitzant eines de terminal com `nmcli` o `netplan`.

![Configuració IP](https://github.com/user-attachments/assets/814be02b-72eb-4080-949a-ea548ac7281b)

Comprovem que les IPs s'han aplicat correctament amb una comanda com:

```
ip a
````

![IPs dels hosts](https://github.com/user-attachments/assets/02725cad-a757-4603-b749-912b4c455b21)

---

## Desactivació del Firewall

Per evitar que el firewall bloquegi el tràfic de logs, desactivem `ufw` temporalment:

```
sudo ufw disable
```

![Desactivar firewall](https://github.com/user-attachments/assets/4d003f67-a7aa-41c5-8639-ddeed38214e7)

---

## Instal·lació de rsyslog

Normalment `rsyslog` ja està instal·lat per defecte a Ubuntu, però per assegurar-nos-ho executem:

```
sudo apt update
sudo apt install rsyslog -y
```

![Instal·lació rsyslog](https://github.com/user-attachments/assets/15559735-4340-4c36-8d2a-0ac76828cd47)

---

# Pas 2: Configuració del servidor (Profe)

Ara configurem la màquina que actuarà com a **servidor de logs**, és a dir, la que rebrà els registres dels altres hosts.

Editem el fitxer de configuració principal de `rsyslog`:

```
sudo nano /etc/rsyslog.conf
```

![Edició rsyslog.conf](https://github.com/user-attachments/assets/f8f893e9-ca0b-4071-a5db-36da1901ec52)

Busquem les següents línies i eliminem el comentari (`#`) per habilitar la recepció de logs via **UDP**:

```
module(load="imudp")
input(type="imudp" port="514")
```

Aquestes línies indiquen a `rsyslog` que escolti missatges entrants al **port 514**, que és el port estàndard per a syslog.

> Si volguéssim utilitzar **TCP**, hauríem d'habilitar també el mòdul `imtcp`.

---

## Reiniciar el servei

Un cop guardada la configuració reiniciem el servei:

```
sudo systemctl restart rsyslog
```

![Reiniciar rsyslog](https://github.com/user-attachments/assets/bd66d1dd-3309-44c8-b6cd-ba75f6b37fb8)

---

# Pas 3: Configuració del client (Alumne)

Ara configurem la màquina client perquè **envii tots els logs al servidor**.

Editem el fitxer de configuració de regles per defecte:

```
sudo nano /etc/rsyslog.d/50-default.conf
```

![Configuració client](https://github.com/user-attachments/assets/fd5a27fd-61de-4f79-84cf-d400db2a5288)

Al final del fitxer afegim la següent línia:

```
*.* @192.168.1.10:514
```

Significat de la configuració:

* `*.*` → enviem **tots els logs de tots els serveis**
* `@` → enviament via **UDP**
* `@@` → enviament via **TCP**
* `192.168.1.10` → IP del servidor de logs
* `514` → port de recepció del servidor

---

## Reiniciar el servei al client

Després de modificar la configuració reiniciem `rsyslog`:

```
sudo systemctl restart rsyslog
```

![Reiniciar rsyslog client](https://github.com/user-attachments/assets/bd66d1dd-3309-44c8-b6cd-ba75f6b37fb8)

---

# Pas 4: Verificació i prova final

Finalment comprovem que el sistema de logs funciona correctament.

## Al servidor (Profe)

Deixem el servidor escoltant els logs en temps real:

```
tail -f /var/log/syslog
```

---

## Al client (Alumne)

Enviem un missatge de prova amb la comanda `logger`:

```
logger "prova d’enviament de logs"
```

I tal com es veu a la captura, ho ha rebut correctament:

![Recepció dels logs](https://github.com/user-attachments/assets/46ea9d4c-5cdd-4f0f-b5a1-86968e57fade)

---

# Conclusió

Amb aquesta configuració hem aconseguit implementar un sistema de **centralització de logs** utilitzant `rsyslog`.

Això permet que un servidor rebi i emmagatzemi els registres de múltiples màquines, facilitant:

* la **monitorització del sistema**
* la **detecció d’errors**
* l’**anàlisi de seguretat**

Aquest tipus de configuració és molt utilitzada en **infraestructures de sistemes i xarxes** per tenir un control centralitzat de tots els esdeveniments del sistema.

---



# Servidor d'Actualitzacions

**Data:** 09/03/26

Aquesta documentació explica com configurar un servidor local d'actualitzacions amb `apt-mirror` i Apache, i com configurar un client per obtenir paquets des d’aquest servidor.

---

## 1. Obrir el servidor

Accedim com a root:

```
sudo su
```

Actualitzem els repositoris:

```
apt update
```

Instal·lem Apache2:

```
apt install apache2
```

![Apache instal·lat](https://github.com/user-attachments/assets/c6d88018-875c-4891-b17b-3fed85095733)

Instal·lem `apt-mirror`:

```
apt install apt-mirror
```

![Apt-mirror instal·lat](https://github.com/user-attachments/assets/9c8c385f-f0db-4e80-bb24-4873e5de5858)

---

## 2. Configurar `apt-mirror`

Editem la configuració:

```
nano /etc/apt/mirror.list
```

* Afegim tots els repositoris que volem descarregar al servidor local.
* D’aquesta manera, els clients no hauran de descarregar els paquets d’internet.
* Per a la prova, comentem tots els altres repositoris i afegim només el de Google Chrome.

![Configuració mirror.list](https://github.com/user-attachments/assets/8dfb681b-44de-4dd7-8897-4c4fade3c069)

Executem la descàrrega dels paquets:

```
apt-mirror
```

![Execució apt-mirror](https://github.com/user-attachments/assets/4cb18ca3-68ee-49cf-93eb-6bfc22c24901)

---

## 3. Configurar Apache

Creem un **softlink** per servir els paquets amb Apache:

```
ln -s /var/spool/apt-mirror/mirror/dl.google.com/linux/chrome/deb /var/www/html/
```

![Softlink creat](https://github.com/user-attachments/assets/0f47648b-d9b5-45e8-a12d-0caf98b9c944)

Verifiquem que el softlink s’ha creat correctament:

![Verificació softlink](https://github.com/user-attachments/assets/a53ace71-5df1-42cd-b8e3-b7348bd4414a)

Comprovem la IP del servidor (en aquest exemple `10.0.2.9`):

![IP del servidor](https://github.com/user-attachments/assets/24cdcfce-a646-42c5-8754-3ff2e09418e3)

---

## 4. Configurar el client

Obrim el fitxer de repositoris del client:

```
nano /etc/apt/sources.list
```

![Sources.list client](https://github.com/user-attachments/assets/abe74f5b-1515-474a-af9f-a00c40a2acb9)

Com que Google Chrome necessita signatura, la importem:

![Signatura Chrome](https://github.com/user-attachments/assets/bd56d477-e673-4cf1-a3c2-771ea4cf20f9)

Fem un `apt update` per comprovar que el client obté els paquets del servidor local:

![Apt update client](https://github.com/user-attachments/assets/05e5dd6b-010a-44a7-ad4f-3a7fc0214330)

Instal·lem el paquet des del servidor:

```
apt install google-chrome-stable
```

![Instal·lació Chrome](https://github.com/user-attachments/assets/feb84d4a-a253-46c7-a5d9-23b3ba70c911)

I tal com es veu a la captura s'ha instal·lat correctament:
<img width="400" height="252" alt="image" src="https://github.com/user-attachments/assets/b496a703-459b-40d3-9ee3-a0b03250142f" />

---

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
