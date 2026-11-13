# Workshop-Docker

> [!NOTE]
> This repository contains the material for a beginner workshop on containers and Docker. The workshop and all of its content are in **German**.

Material für einen Einsteiger-Workshop zu Containern und Docker.

- [Presentation.pdf](Presentation.pdf) – Folien (Quelle: `Presentation.odp`)
- [Cheat-Sheet.md](Cheat-Sheet.md) – die wichtigsten Befehle zum Nachschlagen
- [Übungen](Übungen) – Aufgaben zum Mitmachen
- Lösungen: [Workshop-Docker.Solution](https://github.com/LNA-DEV/Workshop-Docker.Solution)

## Voraussetzungen

- Docker Engine (Linux) oder Docker Desktop (macOS/Windows), inkl. Docker Compose v2
- Eine Linux-Shell (Bash): Linux, macOS-Terminal oder **WSL2** unter Windows
- Internetzugang zu Docker Hub (Images werden während des Workshops heruntergeladen)

Kurzer Test vor dem Workshop:

```bash
docker run --rm hello-world
docker compose version
```

Erscheint „Hello from Docker!“ und eine Compose-Version ≥ 2, kann es losgehen.

## Übungen

| Nr. | Thema | Inhalt |
|-----|-------|--------|
| 01 | Webserver | Fertiges Image (nginx) starten, Ports veröffentlichen, Logs |
| 02 | Dockerfile – Website | Eigenes Image für eine statische Website bauen |
| 03 | Dockerfile – Node-App | Todo-App containerisieren, Build-Cache verstehen |
| 04 | Volumes | Daten der Todo-App dauerhaft speichern |
| 05 | Bind Mounts | Ordner vom Host einbinden, Live-Entwicklung im Container |
| 06 | Compose | Web-App + Redis mit Docker Compose |
| 07 | Networks | Bridge vs. Host, Todo-App + MySQL im eigenen Netzwerk |
| 08 | Freie Übung | Eine beliebige App mit Docker betreiben |

Die Übungen 03, 04, 05 und 07 bauen auf der Todo-App aus Übung 03 auf.

## Quellen

- Die Todo-App in Übung 03 stammt aus [docker/getting-started-app](https://github.com/docker/getting-started-app).
- Die Flask/Redis-App in Übung 06 stammt aus dem [Docker-Compose-Quickstart](https://docs.docker.com/compose/gettingstarted/).
