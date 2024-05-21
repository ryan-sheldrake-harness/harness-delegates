# harness-delegates

Gcloud delegate (with google cloud cli embeded) as well as python 3.8
=====================================================================

Build command - 
docker build -t ryansheldrakeharness/delegate-gcloud-cli:2024-05-20 -f Dockerfile --build-arg TARGETARCH=amd64 --build-arg DELEGATEVERSION=24.04.82804 .

Push command - 
docker push ryansheldrakeharness/delegate-gcloud-cli:2024-05-2

Note  - The delegate.jar need to be in the same directory as the Dockerfile - available upon request from Harness. 

