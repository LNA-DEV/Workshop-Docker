# Container / Docker Cheat-Sheet

## Allgemeines

Container-Technologien verpacken Anwendungen und deren Abhängigkeiten in isolierte Umgebungen. Sie sind leichter und schneller als virtuelle Maschinen und sorgen für eine einheitliche Laufzeitumgebung auf verschiedenen Plattformen.

- **Container vs. VM**: Container teilen sich den Kernel des Hosts, statt ein eigenes Betriebssystem zu starten. Das macht sie im Vergleich zu VMs leichter und schneller.
- **Docker**: Eine der bekanntesten Plattformen für Container. Sie vereinfacht das Erstellen, Verteilen und Betreiben von Containern.

### Begriffe

- **Dockerfile**: Bauanleitung für ein Image
- **Image**: Unveränderliche Vorlage aus mehreren Schichten (Layern) – Dateisystem plus Startbefehl
- **Container**: Laufende (oder gestoppte) Instanz eines Images mit eigener, beschreibbarer Schicht
- **Registry**: Ablage für Images, z. B. Docker Hub oder GitHub Container Registry

## Docker CLI

### Container verwalten

- `docker run <image>`: Container aus einem Image starten. Häufige Optionen:
  - `-d` – im Hintergrund starten
  - `-p 8080:80` – Port 80 des Containers auf Port 8080 des Hosts veröffentlichen
  - `--name <name>` – Namen vergeben
  - `-v <volume>:<pfad>` / `-v "$(pwd)/ordner:<pfad>"` – Volume bzw. Bind Mount einbinden
  - `-e NAME=wert` – Umgebungsvariable setzen
  - `--network <netz>` – Container in einem bestimmten Netzwerk starten
  - `--rm` – Container nach dem Beenden automatisch löschen
  - `-it` – interaktiv mit Terminal (z. B. `docker run -it --rm alpine sh`)
- `docker ps`: Laufende Container anzeigen.
- `docker ps -a`: Alle Container anzeigen (laufend und gestoppt).
- `docker stop <container>` / `docker start <container>`: Container stoppen bzw. wieder starten.
- `docker rm <container>`: Gestoppten Container löschen (`-f` stoppt und löscht in einem Schritt).

### Images verwalten

- `docker images`: Alle lokal verfügbaren Images anzeigen.
- `docker pull <image>[:tag]`: Ein Image aus einer Registry herunterladen.
- `docker build -t <name>:<tag> .`: Ein Image aus dem Dockerfile im aktuellen Verzeichnis bauen.
- `docker history <image>`: Schichten eines Images und ihre Größe anzeigen.
- `docker tag <image> <neuer-name>:<tag>`: Einem Image einen weiteren Namen geben (z. B. für eine Registry).
- `docker login [<registry>]` / `docker push <name>:<tag>`: Bei einer Registry anmelden bzw. ein Image hochladen.
- `docker rmi <image>` (oder `docker image rm <image>`): Ein Image löschen.

### Volumes

- `docker volume create <name>`: Ein Volume erstellen.
- `docker volume ls`: Alle Volumes anzeigen.
- `docker volume inspect <name>`: Details eines Volumes anzeigen.
- `docker volume rm <name>`: Ein Volume löschen (nur möglich, wenn kein Container es verwendet).

### Netzwerke

- `docker network ls`: Alle Netzwerke anzeigen.
- `docker network create <name>`: Neues Bridge-Netzwerk erstellen.
- `docker network connect <netzwerk> <container>`: Container zusätzlich mit einem Netzwerk verbinden.
- `docker network inspect <name>`: Details und verbundene Container anzeigen.

Treiber:

- **bridge** (Standard): Isoliertes Netzwerk auf dem Host. In **eigenen** Bridge-Netzwerken erreichen sich Container über ihren Namen – im Standard-Netzwerk `bridge` nicht.
- **host**: Container nutzt direkt das Netzwerk des Hosts, keine Port-Weiterleitung nötig. Es gibt genau ein Host-Netzwerk.
- **none**: Kein Netzwerk.

### Weitere nützliche Befehle

- `docker exec -it <container> sh`: Shell im laufenden Container öffnen (bei Debian-basierten Images oft auch `bash`).
- `docker logs <container>`: Logs eines Containers anzeigen (`-f` folgt den Logs live).
- `docker inspect <container|image>`: Alle Details eines Containers oder Images anzeigen.
- `docker cp <container>:<pfad> <ziel>`: Dateien zwischen Container und Host kopieren.
- `docker stats`: CPU- und Speicherverbrauch der laufenden Container anzeigen.

### Aufräumen

- `docker system df`: Anzeigen, wie viel Speicher Images, Container und Volumes belegen.
- `docker container prune`: Alle gestoppten Container löschen.
- `docker image prune`: Images ohne Namen (`<none>`) löschen; mit `-a` alle ungenutzten Images.
- `docker volume prune`: Ungenutzte Volumes löschen – **Vorsicht, Daten sind danach weg**.
- `docker system prune`: Gestoppte Container, ungenutzte Netzwerke, Images ohne Namen und den Build-Cache löschen.

## Dockerfile

Ein Dockerfile beschreibt, wie ein Image gebaut wird. Docker arbeitet die Anweisungen beim Build von oben nach unten ab.

### Grundstruktur eines Dockerfiles

```Dockerfile
# Basis-Image mit festem Tag
FROM python:3.14-alpine

# Arbeitsverzeichnis festlegen
WORKDIR /app

# Zuerst nur die Abhängigkeiten kopieren und installieren (Build-Cache!)
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Danach den restlichen Code kopieren
COPY . .

# Dokumentiert den Port, auf dem die Anwendung lauscht
EXPOSE 5000

# Nicht als root laufen
USER nobody

# Startbefehl für den Container
CMD ["python", "app.py"]
```

### Wichtige Dockerfile-Anweisungen

- `FROM`: Basis-Image, auf dem aufgebaut wird.
- `WORKDIR`: Arbeitsverzeichnis für alle folgenden Anweisungen und den Container.
- `COPY`: Kopiert Dateien aus dem Build-Kontext ins Image.
- `RUN`: Führt beim Build einen Befehl aus, z. B. die Installation von Paketen.
- `ENV`: Setzt Umgebungsvariablen.
- `EXPOSE`: Dokumentiert, welche Ports der Container nutzt (veröffentlicht sie aber **nicht** – dafür braucht es `-p`).
- `USER`: Benutzer, unter dem folgende Befehle und der Container laufen.
- `CMD`: Standardbefehl beim Start des Containers (kann bei `docker run` überschrieben werden).
- `ENTRYPOINT`: Fester Startbefehl; `CMD` liefert dann nur noch die Standard-Argumente.
- `HEALTHCHECK`: Befehl, mit dem Docker prüft, ob die Anwendung funktioniert.

### `.dockerignore`

Dateien, die nicht in den Build-Kontext gehören (z. B. `node_modules`, `.git`, lokale Konfiguration), werden in `.dockerignore` eingetragen. Das macht den Build schneller und verhindert, dass Unerwünschtes im Image landet.

### Layer und Build-Cache

- Jede Anweisung (`RUN`, `COPY`, …) erzeugt eine Schicht (Layer). Schichten werden zwischen Images geteilt und gecacht.
- Ändert sich die Eingabe einer Anweisung, werden **diese und alle folgenden** Schichten neu gebaut.
- Daher: Selten Änderndes (Basis-Image, Abhängigkeiten) nach oben, häufig Änderndes (Code) nach unten.
- Aufräumen im **selben** `RUN`, z. B. `RUN apk add --no-cache curl` oder `RUN yarn install && yarn cache clean` – sonst bleibt der Cache in einer früheren Schicht erhalten.

### Multi-Stage Builds

Werkzeuge, die nur zum Bauen gebraucht werden (Compiler, SDKs, Dev-Abhängigkeiten), gehören nicht ins fertige Image. Mit mehreren `FROM`-Stufen wird nur das Ergebnis übernommen:

```Dockerfile
# Stufe 1: bauen
FROM node:24-alpine AS build
WORKDIR /app
COPY package.json yarn.lock ./
RUN yarn install --frozen-lockfile
COPY . .
RUN yarn build

# Stufe 2: nur das Ergebnis ausliefern
FROM nginx:stable-alpine
COPY --from=build /app/dist /usr/share/nginx/html
```

Das fertige Image enthält nur nginx und die gebauten Dateien – kein Node, keine `node_modules`.

## Tags und Registries

- Ein Image-Name hat die Form `[registry/][namespace/]name[:tag]`, z. B. `nginx:1.29`, `ghcr.io/firma/app:2.1.0`. Ohne Registry ist Docker Hub gemeint, ohne Tag `latest`.
- `latest` ist nur ein Name, **keine** Garantie für „die neueste Version“ – und er ändert sich jederzeit. Für reproduzierbare Builds und Deployments feste Tags verwenden (z. B. `node:24-alpine`, `python:3.14-alpine`).
- Image in eine Registry hochladen:

  ```bash
  docker tag todo-app ghcr.io/<benutzer>/todo-app:1.0
  docker login ghcr.io
  docker push ghcr.io/<benutzer>/todo-app:1.0
  ```

- Bekannte Registries: Docker Hub, GitHub Container Registry (`ghcr.io`), GitLab, Azure/AWS/Google Container Registry, Harbor (selbst betrieben).

## Docker Compose

Docker Compose beschreibt Anwendungen aus mehreren Containern (Services) in einer YAML-Datei (`compose.yaml` oder `docker-compose.yaml`) und startet sie mit einem Befehl.

### Grundstruktur einer Compose-Datei

```yaml
services:
  web:
    build: .
    ports:
      - "${WEB_PORT:-8000}:5000"
    environment:
      - DB_HOST=db
    depends_on:
      db:
        condition: service_healthy

  db:
    image: postgres:17
    volumes:
      - db_data:/var/lib/postgresql/data
    environment:
      - POSTGRES_USER=admin
      - POSTGRES_PASSWORD=secret
    healthcheck:
      test: ["CMD", "pg_isready", "-U", "admin"]
      interval: 5s
      retries: 10

volumes:
  db_data:
```

- Eine `version:`-Angabe ist **nicht mehr nötig** und erzeugt nur eine Warnung.
- Compose legt automatisch ein Netzwerk an, in dem sich alle Services über ihren Namen erreichen (`web` erreicht die Datenbank unter `db`).
- `depends_on` steuert die Startreihenfolge; mit `condition: service_healthy` wird auf den `healthcheck` gewartet.
- Variablen wie `${WEB_PORT:-8000}` werden aus der Umgebung oder einer `.env`-Datei im selben Ordner gelesen (`:-8000` ist der Standardwert).
- Tipp: Host-Port 5000 ist auf macOS oft vom AirPlay-Empfänger belegt – lieber 8000 o. Ä. verwenden.

### Häufige Docker-Compose-Befehle

Seit Compose v2 ist Compose ein Teil der Docker CLI: `docker compose` (mit Leerzeichen). Das alte `docker-compose` (v1) wird nicht mehr weiterentwickelt.

- `docker compose up -d`: Services im Hintergrund starten (`--build` baut Images vorher neu).
- `docker compose down`: Services stoppen und Container sowie Netzwerk entfernen (`-v` löscht auch die Volumes).
- `docker compose ps`: Status der Services anzeigen.
- `docker compose logs -f [service]`: Logs aller oder einzelner Services verfolgen.
- `docker compose exec <service> sh`: Shell in einem laufenden Service öffnen.
- `docker compose build`: Images der Services bauen.
- `docker compose pull`: Neuere Versionen der verwendeten Images herunterladen.
- `docker compose config`: Die fertig ausgewertete Konfiguration anzeigen – gut zur Fehlersuche.

## Sicherheit – die wichtigsten Grundregeln

- **Vertrauenswürdige Images** verwenden: offizielle Images, „Verified Publisher“ oder selbst gebaut.
- **Feste Tags** statt `latest`, Basis-Images regelmäßig aktualisieren (neu bauen).
- **Kleine Basis-Images** (z. B. `-alpine`, `-slim`, distroless) – weniger Software, weniger Angriffsfläche.
- **Nicht als root** laufen: `USER` im Dockerfile setzen.
- **Keine Geheimnisse ins Image**: Passwörter und Schlüssel nicht per `COPY` oder `ENV` einbauen, sondern zur Laufzeit übergeben.
- **Images scannen**, z. B. mit `docker scout cves <image>` oder [Trivy](https://trivy.dev/).
- **Nur nötige Ports** veröffentlichen; Datenbanken brauchen meist kein `-p`.

## Docker Desktop und Alternativen

**Docker Desktop** (Windows/macOS/Linux) ist für Privatpersonen, Ausbildung, nicht-kommerzielle Open-Source-Projekte und kleine Unternehmen (weniger als 250 Beschäftigte **und** weniger als 10 Mio. USD Jahresumsatz) kostenlos. Größere Unternehmen benötigen ein kostenpflichtiges Abonnement. Die Docker Engine selbst (unter Linux) ist Open Source und kostenlos.

Alternativen:

- **Podman** / **Podman Desktop**: Docker-kompatible Container-Engine ohne Daemon, kann ohne Root-Rechte laufen (rootless).
- **Rancher Desktop**: Desktop-App mit containerd/nerdctl oder Docker Engine, inkl. Kubernetes.
- **containerd / nerdctl**: Die Runtime, die auch Docker intern verwendet, mit Docker-ähnlicher CLI.
- **CRI-O**: Schlanke Runtime speziell für Kubernetes.
- **LXC/Incus**: System-Container, die sich eher wie leichte VMs verhalten.

### Open Container Initiative (OCI)

Die **Open Container Initiative (OCI)** wurde 2015 unter dem Dach der Linux Foundation gegründet, um offene Standards für Container zu definieren. Die drei Spezifikationen der OCI:

- **Image Specification**: Wie ein Container-Image aufgebaut ist.
- **Runtime Specification**: Wie eine Container-Runtime einen Container aus einem Image startet.
- **Distribution Specification**: Wie Images über eine Registry verteilt werden (Registry-API).

Dadurch lassen sich Images, die mit Docker gebaut wurden, z. B. mit Podman oder in Kubernetes ausführen – und jede OCI-konforme Registry kann sie speichern.

## Links

- [Docker Hub](https://hub.docker.com/)
- [Docker Docs](https://docs.docker.com/)
- [Dockerfile-Referenz](https://docs.docker.com/reference/dockerfile/)
- [Compose-Referenz](https://docs.docker.com/reference/compose-file/)
- [Podman](https://podman.io/)
- [A Beginner-Friendly Introduction to Containers, VMs and Docker by freeCodeCamp](https://www.freecodecamp.org/news/a-beginner-friendly-introduction-to-containers-vms-and-docker-79a9e3e119b/)
