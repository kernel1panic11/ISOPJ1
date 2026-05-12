<div class="content-section">

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

<div class="code-block">logger -i -s -p mail.err "Mensaje de error"</div>

</div>

<div class="content-section">

<h2>Rotación de logs con logrotate</h2>

<p>
Acceso al directorio de configuración.<br>
img/15
</p>

<div class="img-wrap">
    <img class="sp5-img" src="img/15.png" alt="logrotate">
</div>

<p>
Configuración de rsyslog para logrotate.<br>
img/16
</p>

<div class="img-wrap">
    <img class="sp5-img" src="img/16.png" alt="Configuración rsyslog">
</div>

</div>
