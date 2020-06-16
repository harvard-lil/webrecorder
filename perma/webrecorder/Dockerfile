# WR is currently using pywb:develop; this is the digest of develop on 6/16/20 1:109PM
FROM webrecorder/pywb@sha256:9cb1b077058bd5ee9fa2d753c5d80584d2f8dd7d6b93d76998d6c8042fc8ff13

WORKDIR /build

ADD webrecorder/requirements.txt /build/

RUN pip install -U pip \
    && pip install git+https://github.com/FedericoCeratto/bottle-cork.git@94d4017a4d1b0d20328e9283e341bd674df3a18a#egg=bottle-cork \
    && pip install -r requirements.txt

RUN useradd -ms /bin/bash -u 1000 archivist
USER archivist

WORKDIR /code

COPY --chown=archivist:archivist webrecorder /code/

ENTRYPOINT []
