# Übung 05 – Bind Mounts

**Ziel:** Einen Ordner Ihres Rechners in einen Container einbinden und den Unterschied zu Volumes verstehen.

## Teil A – Grundlagen

1. Legen Sie in Ihrem aktuellen Verzeichnis einen Ordner `daten` an.
2. Starten Sie einen `alpine`-Container im Hintergrund, der den Ordner `daten` unter `/data` einbindet.
   (Damit der Container weiterläuft, braucht er einen Befehl, der nicht endet, z. B. `sleep infinity`.)
3. Erstellen Sie **im Container** die Datei `/data/aus-dem-container.txt`.
4. Prüfen Sie **auf Ihrem Rechner**: Liegt die Datei im Ordner `daten`?
5. Erstellen Sie jetzt **auf Ihrem Rechner** eine Datei in `daten` und prüfen Sie, ob der Container sie sieht.
6. Löschen Sie den Container. Was passiert mit dem Ordner `daten`?

## Teil B – Live-Entwicklung mit der Todo-App

Bind Mounts werden oft in der Entwicklung genutzt: Der Code liegt auf dem Rechner, läuft aber im Container – ohne nach jeder Änderung ein Image zu bauen.

1. Stoppen Sie alle Container, die noch Port 3000 belegen.
2. Wechseln Sie in den Ordner der Todo-App (`Übungen/03 - Dockerfile`) und starten Sie:

   ```bash
   docker run -d --name todo-dev -p 3000:3000 \
     -w /app -v "$(pwd):/app" \
     node:24-alpine sh -c "yarn install && yarn run dev"
   ```

3. Erklären Sie jede Option dieses Befehls. Warum wird hier **kein** eigenes Image gebaut?
4. Verfolgen Sie mit `docker logs -f todo-dev`, wie die Abhängigkeiten installiert werden und die App startet.
5. Ändern Sie in `src/static/js/app.js` den Button-Text `Add Item` und laden Sie die Seite neu – ohne Build, ohne Neustart.
6. Räumen Sie auf: Container löschen. Der Ordner `node_modules`, den der Container angelegt hat, gehört unter Linux dem Benutzer `root`. Löschen Sie ihn deshalb ebenfalls per Container:

   ```bash
   docker run --rm -v "$(pwd):/app" alpine rm -rf /app/node_modules
   ```

## Geschafft, wenn …

- Dateien in beide Richtungen zwischen Rechner und Container sichtbar sind,
- eine Code-Änderung an der Todo-App sofort im Browser erscheint.

## Volume oder Bind Mount?

Überlegen Sie zum Abschluss: Wann würden Sie ein Volume (Übung 04) verwenden, wann einen Bind Mount?

<details>
<summary>Tipp</summary>

- Bind Mount: `-v "$(pwd)/daten:/data"` – der Pfad auf dem Host muss **absolut** sein, daher `$(pwd)`.
- Alternative, ausführlichere Schreibweise: `--mount type=bind,src="$(pwd)/daten",dst=/data`
- `docker exec <container> sh -c "echo Hallo > /data/datei.txt"` führt einen Befehl im Container aus.

</details>
