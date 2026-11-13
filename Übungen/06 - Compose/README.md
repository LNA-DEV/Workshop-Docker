# Übung 06 – Docker Compose

**Ziel:** Eine Anwendung aus mehreren Containern (Web-App + Redis) mit einer einzigen Datei beschreiben und starten.

Die Web-App (`app.py`) zählt Seitenaufrufe und speichert den Zähler in Redis. Sie erreicht Redis unter dem Hostnamen `redis`.

## Teil A – Dockerfile

1. Vervollständigen Sie das `Dockerfile` (siehe Kommentare darin).

## Teil B – Compose-Datei

1. Beschreiben Sie in `docker-compose.yaml` zwei Services:
   - `web`: wird aus dem `Dockerfile` in diesem Ordner gebaut, Port 5000 des Containers wird auf Port 8000 Ihres Rechners veröffentlicht
   - `redis`: verwendet das fertige Image `redis:8-alpine`
2. Starten Sie alles mit `docker compose up -d --build`.
3. Öffnen Sie <http://localhost:8000> und laden Sie die Seite mehrmals neu.
4. Sehen Sie sich mit `docker compose ps` und `docker compose logs` den Zustand und die Logs an.
5. Warum findet die Web-App den Redis-Server unter dem Namen `redis`? Schauen Sie sich dazu `docker network ls` an.

## Teil C – Daten behalten

1. Führen Sie `docker compose down` und anschließend wieder `docker compose up -d` aus. Wie steht der Zähler?
2. Redis speichert seine Daten im Container unter `/data`. Ergänzen Sie ein benanntes Volume, damit der Zähler `down`/`up` überlebt.

## Bonus

- **Startreihenfolge:** Sorgen Sie mit `depends_on` und einem `healthcheck` (Befehl `redis-cli ping`) dafür, dass `web` erst startet, wenn Redis bereit ist.
- **Konfiguration:** Machen Sie den Host-Port über eine Variable `WEB_PORT` aus einer `.env`-Datei konfigurierbar (Standard: 8000).

## Geschafft, wenn …

- der Zähler bei jedem Neuladen steigt,
- der Zähler nach `docker compose down` und `up` weiterzählt.

<details>
<summary>Tipp</summary>

- Eine `version:`-Angabe ist in Compose-Dateien nicht mehr nötig (und erzeugt nur eine Warnung).
- `build: .` baut ein Image aus dem Dockerfile im aktuellen Ordner, `image:` verwendet ein fertiges Image.
- Benannte Volumes werden unter dem Schlüssel `volumes:` auf oberster Ebene deklariert.
- `docker compose down -v` löscht auch die Volumes – Vorsicht!

</details>
