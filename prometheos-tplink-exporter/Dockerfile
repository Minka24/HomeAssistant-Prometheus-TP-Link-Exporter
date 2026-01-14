# Build stage: Debian bullseye, Python venv, dumb-init
FROM docker.io/debian:bullseye-slim AS build

RUN apt-get update && \
    apt-get install --no-install-suggests --no-install-recommends --yes \
      wget python3-venv gcc libpython3-dev && \
    rm -rf /var/lib/apt/lists/* && \
    python3 -m venv /venv && \
    /venv/bin/pip install --upgrade pip

RUN /venv/bin/pip install dumb-init

# Make run.sh executable in build stage so final distroless image doesn't need chmod
COPY run.sh /app/run.sh
RUN chmod +x /app/run.sh

# Build venv with requirements and download tempio
FROM build AS build-venv
COPY requirements.txt /requirements.txt
RUN /venv/bin/pip install --disable-pip-version-check -r /requirements.txt

# Install curl and download tempio in build-venv stage
ARG TEMPIO_VERSION
ARG BUILD_ARCH
RUN apt-get update && apt-get install --no-install-suggests --no-install-recommends --yes curl \
  && curl -sSLf -o /usr/bin/tempio "https://github.com/home-assistant/tempio/releases/download/${TEMPIO_VERSION}/tempio_${BUILD_ARCH}" \
  && chmod +x /usr/bin/tempio \
  && apt-get remove --purge --yes curl \
  && rm -rf /var/lib/apt/lists/*

# Final distroless image
FROM gcr.io/distroless/python3-debian11

COPY --from=build /venv/bin/dumb-init /usr/bin/dumb-init
WORKDIR /app
COPY --from=build-venv /venv /venv
COPY tl-sg-prometheus-exporter.py /app/exporter.py
COPY --from=build /app/run.sh /app/run.sh
COPY --from=build-venv /usr/bin/tempio /usr/bin/tempio

# Copy root filesystem
COPY rootfs /

# ENTRYPOINT ["/usr/bin/dumb-init", "--"]
# CMD ["/app/run.sh"]
