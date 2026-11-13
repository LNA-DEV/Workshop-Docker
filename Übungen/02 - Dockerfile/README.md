# Übung 02 – Dockerfile: Statische Website

**Ziel:** Ein eigenes Image bauen, das eine selbst geschriebene HTML-Datei ausliefert.

## Aufgaben

1. Füllen Sie die `index.html` mit Inhalt (z. B. eine Überschrift mit Ihrem Namen).
2. Schreiben Sie das `Dockerfile`:
   - Basis-Image: `nginx` in der Alpine-Variante (kleiner)
   - Kopieren Sie die `index.html` in das Verzeichnis, aus dem nginx Webseiten ausliefert: `/usr/share/nginx/html/`
3. Bauen Sie das Image mit dem Namen und Tag `meine-website:1.0`.
4. Starten Sie einen Container aus Ihrem Image und veröffentlichen Sie ihn auf Port 8080.
5. Ändern Sie die `index.html` und laden Sie die Seite neu. Ändert sich etwas? Warum (nicht)?
6. Bauen Sie das Image als `meine-website:1.1` neu und starten Sie einen neuen Container daraus.

## Geschafft, wenn …

- `docker images` Ihr Image `meine-website` mit beiden Tags anzeigt,
- unter <http://localhost:8080> Ihre eigene Seite erscheint,
- Sie erklären können, warum eine Änderung an der Datei erst nach einem neuen Build sichtbar wird.

<details>
<summary>Tipp</summary>

- `FROM` legt das Basis-Image fest, `COPY <quelle> <ziel>` kopiert Dateien ins Image.
- Gebaut wird mit `docker build -t <name>:<tag> .` – der Punkt am Ende ist der *Build-Kontext* (das aktuelle Verzeichnis).
- Beim Neustart auf Port 8080 muss der alte Container vorher gestoppt sein, sonst ist der Port belegt.

</details>
