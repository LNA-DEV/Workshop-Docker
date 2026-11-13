# Übung 07 – Networks

**Ziel:** Verstehen, wie Container miteinander kommunizieren – und was Docker Compose in Übung 06 automatisch für Sie erledigt hat.

## Teil A – Standard-Bridge vs. eigenes Bridge-Netzwerk

1. Starten Sie zwei `alpine`-Container `a1` und `a2` **ohne** Netzwerk-Option (Befehl: `sleep infinity`).
2. Versuchen Sie, von `a1` aus `a2` per Name zu erreichen: `docker exec a1 ping -c 2 a2`. Was passiert?
3. Finden Sie mit `docker inspect` die IP-Adresse von `a2` heraus und pingen Sie diese.
4. Erstellen Sie ein eigenes Bridge-Netzwerk `mein-netz`.
5. Starten Sie zwei Container `b1` und `b2` in diesem Netzwerk und pingen Sie `b2` von `b1` aus **per Name**.
6. Verbinden Sie `a1` nachträglich mit `mein-netz` (`docker network connect`). Kann `a1` jetzt `b1` per Name erreichen?

## Teil B – Todo-App mit MySQL

Die Todo-App aus Übung 03 kann statt SQLite auch MySQL verwenden. Sie liest dazu die Umgebungsvariablen `MYSQL_HOST`, `MYSQL_USER`, `MYSQL_PASSWORD` und `MYSQL_DB`.

1. Erstellen Sie ein Netzwerk `todo-netz`.
2. Starten Sie einen MySQL-Container (`mysql:8.4`) in diesem Netzwerk:
   - Name `mysql`
   - Volume `todo-mysql` unter `/var/lib/mysql`
   - Umgebungsvariablen `MYSQL_ROOT_PASSWORD=geheim` und `MYSQL_DATABASE=todos`
3. Warten Sie, bis in `docker logs mysql` die Meldung `ready for connections` mit `port: 3306` erscheint (der erste Start dauert etwas).
4. Starten Sie die Todo-App im selben Netzwerk, veröffentlichen Sie Port 3000 und setzen Sie die passenden `MYSQL_*`-Variablen.
5. Prüfen Sie in den Logs der App, dass sie mit MySQL verbunden ist, und legen Sie ein paar Todos an.
6. Schauen Sie direkt in die Datenbank:

   ```bash
   docker exec -it mysql mysql -u root -p todos -e "SELECT * FROM todo_items;"
   ```

## Teil C – Host-Netzwerk

Im Host-Netzwerk teilt sich der Container den Netzwerk-Stack mit Ihrem Rechner – es gibt keine Isolation und keine Port-Weiterleitung.

1. Lassen Sie sich mit `docker network ls` alle Netzwerke anzeigen. Das Netzwerk `host` existiert bereits – es gibt nur eines davon.
2. Vergleichen Sie die Netzwerk-Schnittstellen:

   ```bash
   docker run --rm alpine ip addr
   docker run --rm --network host alpine ip addr
   ```

3. Starten Sie einen nginx-Container mit `--network host` – **ohne** `-p`. Unter welcher Adresse ist er erreichbar?

> Docker Desktop (macOS/Windows): Das Host-Netzwerk muss dort erst in den Einstellungen aktiviert werden und verhält sich anders als unter Linux. Teil C funktioniert am besten auf einem Linux-Host.

## Bonus

Beschreiben Sie Teil B als `compose.yaml` (Todo-App aus `../03 - Dockerfile` bauen + MySQL). Sorgen Sie mit einem `healthcheck` und `depends_on` dafür, dass die App erst startet, wenn MySQL bereit ist. Welches Netzwerk legt Compose an?

## Geschafft, wenn …

- Sie erklären können, warum Namensauflösung im eigenen Bridge-Netzwerk funktioniert, im Standard-Bridge-Netzwerk aber nicht,
- die Todo-App ihre Daten in MySQL speichert,
- Sie den Unterschied zwischen Bridge- und Host-Netzwerk beschreiben können.

<details>
<summary>Tipp</summary>

- `docker network create <name>` erzeugt ein Bridge-Netzwerk (Standardtreiber).
- `docker run --network <name> ...` startet einen Container direkt in einem Netzwerk.
- `docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' a2` gibt nur die IP-Adresse aus.
- Umgebungsvariablen setzen Sie mit `-e NAME=wert`.
- Bricht die Todo-App mit einem Timeout ab, war MySQL noch nicht bereit – einfach erneut starten.

</details>
