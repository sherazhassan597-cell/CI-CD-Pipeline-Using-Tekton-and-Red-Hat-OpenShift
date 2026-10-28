# CI-CD-Pipeline-Using-Tekton-and-Red-Hat-OpenShift
Project Overview

This project demonstrates the implementation of a CI/CD pipeline using Tekton Pipelines on Red Hat OpenShift.
The pipeline builds and deploys the sample Go-based Vote API application from the official OpenShift Pipelines tutorial.
Project Status: Successfully executed and deployed.

##Technologies Used
Red Hat OpenShift
Tekton Pipelines
Kubernetes
Buildah
Git and GitHub
OpenShift Internal Image Registry
OpenShift CLI (oc)
Tekton CLI (tkn)
CI/CD Workflow
GitHub Repository → Fetch Source Code → Build Container Image → Push to OpenShift Registry → Apply Kubernetes Manifests → Update Deployment

Pipeline Tasks
fetch-repository: Retrieves the application source code from GitHub.
build-image: Builds the container image using Buildah and pushes it to the internal OpenShift registry.
apply-manifests: Applies the required Kubernetes manifests.
update-deployment: Updates the application deployment with the built image.
Deployment Configuration
OpenShift project: sheraz-dev
Pipeline: build-and-deploy
Application deployment: pipelines-vote-api
Container image: image-registry.openshift-image-registry.svc:5000/sheraz-dev/vote-api:latest
Troubleshooting Experience
During implementation, the image build succeeded but pushing the image to the registry failed because the pipeline referenced an outdated external registry hostname.
I investigated the pipeline logs, verified the active OpenShift project, corrected the container image destination to use the internal OpenShift registry, and executed the pipeline again.
The subsequent PipelineRun completed successfully, with all four tasks reporting success.
Results
Successfully executed the Tekton CI/CD pipeline.
Built and pushed the container image.
Deployed the application on OpenShift.
Verified the deployment was ready (1/1).
Created an OpenShift Route for the application.

#Commands Used.
oc create -f https://raw.githubusercontent.com/openshift/pipelines-tutorial/master/01_pipeline/01_apply_manifest_task.yaml
oc create -f https://raw.githubusercontent.com/openshift/pipelines-tutorial/master/01_pipeline/02_update_deployment_task.yaml
oc create -f https://raw.githubusercontent.com/openshift/pipelines-tutorial/master/01_pipeline/04_pipeline.yaml

tkn pipeline start build-and-deploy \
  --prefix-name build-and-deploy-api-pipelinerun \
  -n sheraz-dev \
  -w name=shared-workspace,volumeClaimTemplateFile=https://raw.githubusercontent.com/openshift/pipelines-tutorial/master/01_pipeline/03_persistent_volume_claim.yaml \
  -p deployment-name=pipelines-vote-api \
  -p git-url=https://github.com/openshift/pipelines-vote-api.git \
  -p IMAGE=image-registry.openshift-image-registry.svc:5000/sheraz-dev/vote-api:latest \
  --use-param-defaults
