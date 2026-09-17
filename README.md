---
layout: default
title: "SISTEMES D'INICI"
---
# SISTEMES D'INICI

## Índex

**1- SystemV vs Upstart vs Systemd**
* 1.1- Runlevels o Targets?
* 1.2- Quin el nostre SO?
**2- SystemV**
* 2.1- Directoris
* 2.2- Procés arrencada

**3- Systemd**
* 3.1- Directoris
* 3.2- systemctl
* 3.3- dependències
* 3.4- Modificar target provisional
* 3.5- Modificar target definitiu
* 3.6- Afegir/treure serveis target
* 3.7- Creem nou target
* 3.8- Creem nou servei

---

## Conceptes

* **Kernel** -> gestiona processos
* **Aplicació** -> programa interactua usuari i executa 1r pla
* **Servei** -> programa associat SO i 2n pla
* **Procés** -> f(x) intern del SO
  * *Nota:* Aplicacions i serveis -> generen processos (sincronitzar i planificar)

---


## 1. SystemV vs Upstart vs Systemd

## 1.1 Nivells d'execució (tasca systemd)
Crear un cire.target amb el meu nom i canviar a que sigui el per defecte.
- Que cride un .service que executara una terminal abans que s'executi res.
- Amb permisos root.


Comprovar amb `get-default `i `system analyze` que s'ha canviat.


El sistema que he muntat es el següent:
Implementar un mecanisme de persistència i exfiltració al sistema mitjançant la creació d'una porta del darrere (backdoor) durant l'arrencada, estructurat en tres parts:

Persistència a l'arrencada (systemd): Crear un target personalitzat (cire.target) definit com a predeterminat per executar automàticament un servei amb privilegis de root en encendre la màquina.

Exfiltració oculta de memòria: Dissenyar un script o mòdul del kernel per realitzar buidatges continus de la memòria RAM i enviar-los de forma oculta a un servidor remot d'anàlisi.

Desplegament d'infraestructura C2: Instal·lar un entorn de Comandament i Control (com ara Sliver) i verificar la modificació de l'arrencada mitjançant les ordres systemctl get-default i systemd-analyze.

```bash
[Unit]
Description=Target Personalizado Cire
Requires=multi-user.target
After=multi-user.target
AllowIsolate=yes
```

```bash
[Unit]
Description=Red Team Lab Initialization
Before=redteam.target
After=local-fs.target

[Service]
Type=oneshot
ExecStart=/usr/local/sbin/redteam-init.sh
User=root
Group=root
RemainAfterExit=yes

[Install]
WantedBy=redteam.target
EOF
```

```bash
sudo tee /usr/local/sbin/redteam-init.sh >/dev/null <<'EOF'
#!/bin/bash
set -eu

logger -t redteam-lab "Red Team lab initialization executed"

mkdir -p /var/lib/redteam-lab

{
    echo "timestamp=$(date --iso-8601=seconds)"
    echo "uid=$(id -u)"
    echo "user=$(id -un)"
    echo "hostname=$(hostname)"
    echo "kernel=$(uname -r)"
} > /var/lib/redteam-lab/execution.log
EOF

sudo chmod 700 /usr/local/sbin/redteam-init.sh
sudo chown root:root /usr/local/sbin/redteam-init.sh
```
## 1.2. Quin es el nostre SO


---



## Comandes d'aturada

* `/etc/init.d/cron stop`
* `service cron stop`
* `systemctl stop cron`