ARG BUILD_TARGET=${BUILD_TARGET}

FROM python:3.11-slim-bullseye AS runtime-base

# Install dependencies

RUN export DEBIAN_FRONTEND=noninteractive \
    && apt update \
    && apt install --assume-yes --quiet \
    gdal-bin \
    gettext \
    git \
    less \
    postgresql-client \
    screen \
    && apt --assume-yes --quiet autoremove \
    && apt clean \
    && rm -rf /var/lib/apt/lists/*
ENV APP_HOME=/opt/app \
    PIP_DISABLE_PIP_VERSION_CHECK=1 \
    APP_VENV=/opt/venv \
    PIP_NO_CACHE_DIR=1
ENV PATH=${APP_VENV}/bin:${PATH}
RUN python3 -m venv ${APP_VENV}

WORKDIR ${APP_HOME}

FROM runtime-base AS runtime-dist
RUN pip install poetry
COPY . ${APP_HOME}
RUN poetry install  --no-root --with release
RUN poetry build -f wheel

FROM runtime-base AS runtime-release
COPY --from=runtime-dist ${APP_HOME}/dist/*.whl ${APP_HOME}/dist/
RUN pip install ${APP_HOME}/dist/*.whl
CMD ["gunicorn", "--version"]

FROM runtime-dist AS runtime-testing
CMD ["poetry", "run", "pytest"]


FROM runtime-${BUILD_TARGET}

