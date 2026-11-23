# Image de base Python légère
FROM python:3.12-slim

# Répertoire de travail de l'application
WORKDIR /app

# Copier la liste des dépendances, puis les installer
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copier le code de l'application (app.py, templates...)
COPY . .

# Port sur lequel l'application écoute
EXPOSE 5000

# Démarrer l'application avec gunicorn, accessible depuis l'extérieur
CMD ["gunicorn", "--bind", "0.0.0.0:5000", "app:app"]