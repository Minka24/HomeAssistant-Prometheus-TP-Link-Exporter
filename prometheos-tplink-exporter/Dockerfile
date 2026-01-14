# Single-stage image: Debian bullseye, Python venv, dumb-init
FROM docker.io/debian:bullseye-slim

RUN apt-get update && \
    apt-get install --no-install-suggests --no-install-recommends --yes \
      wget python3-venv gcc libpython3-dev dumb-init gettext && \
    rm -rf /var/lib/apt/lists/* && \
    python3 -m venv /venv && \
    /venv/bin/pip install --upgrade pip

WORKDIR /app

# Install Python requirements
COPY requirements.txt /requirements.txt
RUN /venv/bin/pip install --disable-pip-version-check -r /requirements.txt

# Copy application and scripts
COPY tl-sg-prometheus-exporter.py /app/exporter.py
COPY run.sh /app/run.sh
COPY tl-sg-prometheus-exporter.yaml /app/tl-sg-prometheus-exporter.yaml.template
RUN chmod +x /app/run.sh

# Use envsubst to replace environment variables in the YAML config at container startup
ENTRYPOINT ["sh", "-c", "envsubst < /app/tl-sg-prometheus-exporter.yaml.template > /app/tl-sg-prometheus-exporter.yaml && exec /venv/bin/python3 /app/exporter.py --config /app/tl-sg-prometheus-exporter.yaml"]
