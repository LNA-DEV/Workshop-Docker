# Erstellen sie eine Dockerfile für app.py
# Notwendig: pip install -r requirements.txt
# Deswegen muss diese Datei im Container sein
# Exposen sie den Port 5000
# Ersetzen sie XXX mit einem sinnvollen Basis-Image
# Tipp: Um den Package-Manager apk zu verwenden wird eine Alpine Version des Images benötigt!

FROM XXX 
ENV FLASK_APP=app.py
ENV FLASK_RUN_HOST=0.0.0.0
RUN apk add --no-cache gcc musl-dev linux-headers

CMD ["flask", "run", "--debug"]
