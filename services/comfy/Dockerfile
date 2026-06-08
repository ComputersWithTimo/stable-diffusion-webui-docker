FROM pytorch/pytorch:2.12.0-cuda12.6-cudnn9-runtime

ENV DEBIAN_FRONTEND=noninteractive PIP_PREFER_BINARY=1

RUN apt-get update && apt-get install -y git pipx && apt-get clean

ENV ROOT=/stable-diffusion
RUN --mount=type=cache,target=/root/.cache/pip \
  git clone https://github.com/comfyanonymous/ComfyUI.git ${ROOT} && \
  cd ${ROOT} && \
  git checkout master && \
  python3 -m venv /venv && \
  /venv/bin/pip install -r requirements.txt && \
  /venv/bin/python -m pip install matrix-nio


RUN /venv/bin/pip install -U --pre comfyui-manager

WORKDIR ${ROOT}
COPY . /docker/
RUN chmod u+x /docker/entrypoint.sh && cp /docker/extra_model_paths.yaml ${ROOT}

ENV NVIDIA_VISIBLE_DEVICES=all PYTHONPATH="${PYTHONPATH}:${PWD}" CLI_ARGS=""
EXPOSE 7860
ENTRYPOINT ["/docker/entrypoint.sh"]
CMD /venv/bin/python -u main.py --listen --port 7860 --enable-manager ${CLI_ARGS}
