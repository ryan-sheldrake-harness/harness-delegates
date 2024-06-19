# harness-delegates

Gcloud delegate (with google cloud cli embeded) as well as python 3.8
=====================================================================

Build command - 
docker build -t ryansheldrakeharness/delegate-aws-cdk-cli -f Dockerfile --build-arg TARGETARCH=amd64 --build-arg DELEGATEVERSION=24.04.82804 .

Push command - 
docker push ryansheldrakeharness/delegate-aws-cdk-cli

Note  - The delegate.jar need to be in the same directory as the Dockerfile - available upon request from Harness. 

