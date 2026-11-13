# Übung 06: Vervollständigen Sie das Dockerfile für app.py
# - Ersetzen Sie XXX durch ein passendes Basis-Image (Python 3.14, Alpine-Variante)
# - Kopieren Sie requirements.txt ins Image und installieren Sie die Abhängigkeiten:
#   pip install -r requirements.txt
# - Kopieren Sie danach den restlichen Code (Reihenfolge beachten -> Build-Cache, Übung 03!)
# - Exposen Sie den Port 5000

FROM XXX
WORKDIR /code
ENV FLASK_APP=app.py
ENV FLASK_RUN_HOST=0.0.0.0

CMD ["flask", "run", "--debug"]
