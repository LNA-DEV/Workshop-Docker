# Übung 01 – Webserver

**Ziel:** Ein fertiges Image von Docker Hub herunterladen, als Container starten und erreichbar machen.

## Aufgaben

1. Machen Sie sich mit den Befehlen aus den Folien vertraut: `docker pull`, `docker images`, `docker run`, `docker ps`. Zu jedem Befehl gibt es eine Hilfe, z. B. `docker run --help`.
2. Laden Sie das offizielle Image `nginx` herunter.
3. Starten Sie einen nginx-Container
   - im Hintergrund,
   - mit dem Namen `mein-nginx`,
   - und veröffentlichen Sie Port 80 des Containers auf Port 8080 Ihres Rechners.
4. Öffnen Sie <http://localhost:8080> im Browser.
5. Sehen Sie sich die Logs des Containers an und laden Sie die Seite im Browser neu. Was passiert?
6. Stoppen und löschen Sie den Container.

## Geschafft, wenn …

- `docker ps` Ihren Container `mein-nginx` mit `0.0.0.0:8080->80/tcp` anzeigt,
- der Browser „Welcome to nginx!“ anzeigt,
- Ihre Seitenaufrufe in den Logs auftauchen.

<details>
<summary>Tipp</summary>

- `-d` startet einen Container im Hintergrund
- `-p <host-port>:<container-port>` veröffentlicht einen Port
- `--name` vergibt einen Namen, über den Sie den Container später ansprechen können
- `docker logs -f <name>` folgt den Logs (beenden mit `Strg+C`)

</details>
