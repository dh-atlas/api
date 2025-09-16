# Multi-stage build per ridurre la dimensione finale dell'immagine
FROM python:3.10-slim as builder

# Variabili di ambiente per ottimizzare Python durante la build
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PIP_NO_CACHE_DIR=1 \
    PIP_DISABLE_PIP_VERSION_CHECK=1

WORKDIR /build

# Installa solo le dipendenze necessarie per la build in un singolo layer
# Raggruppiamo update, install e cleanup per ridurre i layer
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Copia prima solo requirements per sfruttare la cache Docker
# Se requirements.txt non cambia, questo layer viene riutilizzato
COPY ./config/requirements.txt .

# Installa le dipendenze Python in un virtual environment
# Questo ci permette di copiare solo ciò che serve nel container finale
RUN python -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"
RUN pip install --no-cache-dir --upgrade pip setuptools wheel \
    && pip install --no-cache-dir -r requirements.txt

# Stage finale: immagine pulita e leggera
FROM python:3.10-slim as runtime

# Variabili di ambiente per il runtime
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PATH="/opt/venv/bin:$PATH"

# Crea un utente non-root per sicurezza
# Questo evita di eseguire l'applicazione come root
RUN groupadd -r ramose && useradd -r -g ramose ramose

WORKDIR /app

# Copia solo il virtual environment dal builder stage
# Questo esclude tutti gli strumenti di build che non servono a runtime
COPY --from=builder /opt/venv /opt/venv

# Cambia ownership dei file all'utente ramose
RUN chown -R ramose:ramose /app

# Cambia all'utente non-root prima di eseguire l'applicazione
USER ramose

# Espone la porta API
EXPOSE 8080

# Configura l'health check per monitorare lo stato dell'applicazione
# Questo aiuta Docker/Kubernetes a capire se il container è sano
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
    CMD python -c "import urllib.request; urllib.request.urlopen('http://localhost:8080/health').read()" || exit 1

# Avvio dell'applicazione
# Usa exec form per gestire correttamente i segnali di sistema
CMD ["python3", "-m", "ramose", "-s", "/config/config.hf", "-w", "0.0.0.0:8080"]