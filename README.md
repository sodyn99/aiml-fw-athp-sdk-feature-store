# feature-store


`tools/kubeflow/Dockerfile.pipeline`

```dockerfile
FROM python:3.8-slim
RUN apt-get update && \
    apt-get install -y git
COPY requirements_pipeline.txt .
RUN pip3 install -r requirements_pipeline.txt

RUN git clone "https://github.com/sodyn99/aiml-fw-athp-sdk-feature-store.git" /SDK/featurestoresdk_main
RUN git clone -b g-release "https://gerrit.o-ran-sc.org/r/aiml-fw/athp/sdk/model-storage" /SDK/modelmetricssdk_main

RUN pip3 install /SDK/featurestoresdk_main/.
RUN pip3 install /SDK/modelmetricssdk_main/.
RUN mkdir -p /app_run
WORKDIR /app_run
```

`bin/build_default_pipeline_image.sh`

```bash
sudo buildctl --addr=nerdctl-container://buildkitd build \
    --frontend dockerfile.v0 \
    --opt filename=Dockerfile.pipeline \
    --local dockerfile=tools/kubeflow \
    --local context=tools/kubeflow \
    --output type=oci,name=traininghost/pipelineimage:sungjin | sudo nerdctl load --namespace k8s.io
```

