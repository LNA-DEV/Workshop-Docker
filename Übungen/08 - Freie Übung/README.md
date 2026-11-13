# Übung 08 – Freie Übung

**Ziel:** Das Gelernte an einer echten Anwendung anwenden.

Installieren Sie eine **beliebige** App mit Docker – am besten mit Docker Compose.

## Beispiele

- [Authentik](https://goauthentik.io/) – Identity Provider (anspruchsvoll, mehrere Container)
- [Nginx Proxy Manager (NPM)](https://nginxproxymanager.com/) – Reverse Proxy mit Web-Oberfläche
- [Home Assistant](https://www.home-assistant.io/) – Hausautomatisierung
- [Jellyfin](https://jellyfin.org/) – Mediaserver
- [Nextcloud](https://nextcloud.com/) – Dateien & Zusammenarbeit

## Vorgehen

1. Suchen Sie in der Dokumentation des Projekts nach der Installation mit Docker bzw. einer Beispiel-`compose.yaml`.
2. Lesen Sie die Datei, bevor Sie sie starten: Welche Images, Ports, Volumes und Umgebungsvariablen werden verwendet?
3. Starten Sie die App und prüfen Sie Logs und Erreichbarkeit.
4. Prüfen Sie: Überleben Ihre Daten ein `docker compose down` und `up`?

## Worauf Sie achten sollten

- Verwenden Sie Images aus vertrauenswürdigen Quellen (offizielle Images, „Verified Publisher“ oder das Projekt selbst).
- Verwenden Sie feste Versions-Tags statt `latest`.
- Ersetzen Sie Beispiel-Passwörter aus der Dokumentation.
