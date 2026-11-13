# Übung 04 – Volumes

**Ziel:** Verstehen, dass Daten im Container beim Löschen verloren gehen – und sie mit einem Volume dauerhaft speichern.

Sie brauchen das Image `todo-app` aus Übung 03.

## Teil A – Daten gehen verloren

1. Starten Sie einen Container aus `todo-app` (Port 3000) und legen Sie ein paar Todos an.
2. Löschen Sie den Container und starten Sie einen neuen aus demselben Image.
3. Sind Ihre Todos noch da? Warum nicht?

## Teil B – Daten mit einem Volume speichern

Die Todo-App speichert ihre Daten in einer SQLite-Datei unter `/etc/todos/todo.db`.

1. Erstellen Sie ein Volume mit dem Namen `todo-db`.
2. Starten Sie einen Container aus `todo-app` und binden Sie das Volume unter `/etc/todos` ein.
3. Legen Sie ein paar Todos an.
4. Löschen Sie den Container und starten Sie einen neuen – wieder mit dem Volume.
5. Sind die Todos noch da?

## Teil C – In das Volume hineinschauen

1. Zeigen Sie mit `docker volume inspect todo-db` Details zum Volume an. Wo liegen die Daten?
2. Verbinden Sie sich mit `docker exec` in den laufenden Container und sehen Sie sich `/etc/todos` an.
3. Ein Volume kann auch in einen anderen Container eingebunden werden. Starten Sie einen kurzlebigen `alpine`-Container, der das Volume einbindet und den Inhalt auflistet.
4. Räumen Sie auf: Container und Volume löschen.

## Geschafft, wenn …

- Ihre Todos das Löschen und Neuerstellen des Containers überleben,
- Sie erklären können, warum das in Teil A nicht funktioniert hat.

<details>
<summary>Tipp</summary>

- `-v <volume-name>:<pfad-im-container>` bindet ein Volume ein.
- `docker exec -it <container> sh` öffnet eine Shell im Container (beenden mit `exit`).
- `docker run --rm ...` löscht den Container automatisch, sobald er sich beendet.
- Ein Volume lässt sich nur löschen, wenn kein Container es mehr verwendet.

</details>
