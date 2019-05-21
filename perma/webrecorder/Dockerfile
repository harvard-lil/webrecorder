# WR is currently using pywb:develop; this is the digest of develop on 5/21/19 11:16AM
FROM webrecorder/pywb@sha256:5198abb159b1bf9af740a546a4a38e00a12f79398deb7f1d6376c318bb5f0014

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
