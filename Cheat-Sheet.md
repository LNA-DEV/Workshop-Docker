# Container / Docker Cheat-Sheet

## Allgemeines

Container-Technologien ermöglichen es, Anwendungen und deren Abhängigkeiten in isolierte Umgebungen zu packen. Sie sind leichter und schneller als virtuelle Maschinen und fördern eine konsistente Laufzeitumgebung über verschiedene Plattformen hinweg.

- **Container vs. VM**: Container teilen sich den Host-Kernel, was sie im Vergleich zu VMs leichter und schneller macht. 
- **Docker**: Eine der bekanntesten Plattformen für Containerisierung, die das Erstellen, Verteilen und Verwalten von Containern vereinfacht.

## Docker CLI

Mit der Docker CLI können Container-Operationen von der Kommandozeile aus durchgeführt werden. Hier sind einige häufige Befehle:

### Container verwalten

- `docker run <image>`: Container aus einem Image starten.
- `docker ps`: Liste laufender Container anzeigen.
- `docker ps -a`: Liste aller Container (laufend und gestoppt) anzeigen.
- `docker stop <container>`: Container stoppen.
- `docker rm <container>`: Container entfernen.

### Images verwalten

- `docker images`: Alle lokal verfügbaren Images anzeigen.
- `docker pull <image>`: Ein Image aus einem Registry herunterladen.
- `docker rmi <image>`: Ein Image löschen.

### Volumes

- `docker volume create <name>`: Ein Volume erstellen.
- `docker volume ls`: Liste aller Volumes anzeigen.
- `docker volume rm <name>`: Ein Volume löschen.

### Netzwerk

- `docker network ls`: Alle Netzwerke anzeigen.
- `docker network create <name>`: Neues Netzwerk erstellen.
- `docker network connect <network> <container>`: Container mit einem Netzwerk verbinden.

### Weitere nützliche Befehle

- `docker exec -it <container> <command>`: Befehl im laufenden Container ausführen.
- `docker logs <container>`: Logs eines Containers anzeigen.
- `docker inspect <container>`: Details eines Containers oder eines Images anzeigen.

## Dockerfile

Ein Dockerfile definiert, wie ein Docker-Image gebaut wird, indem es Anweisungen enthält, die Docker beim Erstellen des Images abarbeitet.

### Grundstruktur eines Dockerfiles

```Dockerfile
# Basis-Image festlegen
FROM <base-image>

# Maintainer-Info
LABEL maintainer="<your-email>"

# Arbeitsverzeichnis festlegen
WORKDIR /app

# Abhängigkeiten kopieren und installieren
COPY requirements.txt .
RUN pip install -r requirements.txt

# Anwendung kopieren
COPY . .

# Exponierte Ports
EXPOSE 5000

# Startbefehl für den Container
CMD ["python", "app.py"]
```

### Wichtige Dockerfile-Anweisungen

- `FROM`: Definiert das Basis-Image, auf dem aufgebaut wird.
- `COPY`: Kopiert Dateien vom lokalen Rechner in das Image.
- `RUN`: Führt Befehle aus, z. B. Installation von Paketen.
- `EXPOSE`: Deklariert Ports, die der Container nutzen wird.
- `CMD`: Legt den Standardbefehl fest, der beim Start des Containers ausgeführt wird.

## Docker Compose

Docker Compose ermöglicht es, mehrere Container-Umgebungen (Services) mit einer einzigen YAML-Datei zu definieren und zu starten.

### Grundstruktur einer docker-compose.yml-Datei

```yaml
version: '3'

services:
  web:
    image: my-web-app:latest
    ports:
      - "5000:5000"
    volumes:
      - .:/app
    environment:
      - ENV=production

  db:
    image: postgres:13
    volumes:
      - db_data:/var/lib/postgresql/data
    environment:
      - POSTGRES_USER=admin
      - POSTGRES_PASSWORD=secret

volumes:
  db_data:
```

### Häufige Docker Compose-Befehle

- `docker-compose up`: Startet die im YAML-File definierten Services.
- `docker-compose down`: Stoppt und entfernt alle gestarteten Services.
- `docker-compose ps`: Zeigt den Status der definierten Services an.
- `docker-compose logs`: Zeigt die Logs aller oder spezifischer Services an.

## Alternativen zu Docker

Docker ist zwar die am weitesten verbreitete Container-Plattform, aber es gibt einige Alternativen, die je nach Anwendungsfall eine gute Wahl sein können.

- **Podman**: Ein Docker-kompatibler Container-Engine, der keine Daemons benötigt und Root-frei arbeitet.
- **CRI-O**: Ein Kubernetes-orientiertes Tool, das direkt die Container-Runtime-Interface (CRI) verwendet und OCI-konform ist.
- **LXC/LXD**: Bietet eine leichtgewichtigere Containerisierung und wird häufig bei Systemcontainern verwendet.

### Open Container Initiative (OCI)

Die **Open Container Initiative (OCI)** ist eine offene Governance-Struktur, die gegründet wurde, um Industriestandards für Container-Formate und -Runtimes zu definieren. Die wichtigsten Projekte der OCI:

- **OCI Image Format**: Ein Standard, der definiert, wie Container-Images strukturiert und verteilt werden.
- **OCI Runtime Specification**: Definiert, wie ein Container-Laufzeit-Umgebung verhalten soll.

Diese Standards fördern die Kompatibilität zwischen verschiedenen Container-Technologien und erleichtern die Interoperabilität.