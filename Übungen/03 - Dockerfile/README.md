# Übung 03 – Dockerfile: Node-App

**Ziel:** Eine echte Anwendung (Todo-App mit Node.js) in ein Image verpacken und verstehen, wie der Build-Cache funktioniert.

Die Todo-App begleitet Sie auch in den Übungen 04, 05 und 07.

## Teil A – App containerisieren

1. Schreiben Sie das `Dockerfile`:
   - Basis-Image: `node:24-alpine`
   - Arbeitsverzeichnis im Container: `/app`
   - Kopieren Sie den Code ins Image.
   - Installieren Sie die Abhängigkeiten mit `yarn install --production`.
   - Die App lauscht auf Port 3000 und wird mit `node src/index.js` gestartet.
2. Bauen Sie das Image mit dem Namen `todo-app`.
3. Starten Sie einen Container im Hintergrund und veröffentlichen Sie Port 3000.
4. Öffnen Sie <http://localhost:3000> und legen Sie ein paar Todos an.

## Teil B – Build-Cache

1. Ändern Sie in `src/static/js/app.js` den Text `No items yet! Add one above!` (z. B. ins Deutsche).
2. Bauen Sie das Image neu und beobachten Sie die Ausgabe: Welche Schritte sind `CACHED`, welche laufen neu? Läuft `yarn install` erneut?
3. Bauen Sie das `Dockerfile` so um, dass die Abhängigkeiten **nur dann** neu installiert werden, wenn sich `package.json` oder `yarn.lock` ändern.
4. Ändern Sie den Text noch einmal und bauen Sie neu. Jetzt sollte der `yarn install`-Schritt `CACHED` sein.
5. Bonus: Sehen Sie sich mit `docker history todo-app` die Schichten (Layer) Ihres Images und deren Größe an.

## Geschafft, wenn …

- die Todo-App unter <http://localhost:3000> läuft und Ihren geänderten Text zeigt,
- nach einer reinen Code-Änderung der Schritt `yarn install` beim Build als `CACHED` erscheint.

<details>
<summary>Tipp zu Teil B</summary>

Jede Anweisung im Dockerfile erzeugt eine Schicht. Ändert sich die Eingabe einer Anweisung (z. B. eine kopierte Datei), werden **diese und alle folgenden** Schichten neu gebaut.
Kopieren Sie also zuerst nur die Dateien, die für die Installation nötig sind, installieren Sie dann – und kopieren Sie erst danach den restlichen Code.

</details>

> Hinweis: Die Datei `.dockerignore` sorgt dafür, dass ein lokaler `node_modules`-Ordner nicht ins Image kopiert wird.
