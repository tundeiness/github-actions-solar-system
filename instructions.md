Navigate to your GitHub account and use github-actions-solar-system repository within feature/workflow branch
Explore and modify the workflow file named solar-system.yml

Do the following:
Append a new job with id as dev-deploy

a. This job should execute on this operating system - ubuntu-latest
b. This job should run after docker job.
c. The job should be associated with the development environment
d. Add the following steps,

Step 1: Utilize the helm/kind-action@v1 action to install the Kubernetes cluster.
Step 2: Use the run command to execute the following commands:
sleep 60
kubectl -n kube-system get pod
set +e
REPO_OWNER_LC="${GITHUB_REPOSITORY_OWNER,,}"
kubectl run solar --image ghcr.io/${REPO_OWNER_LC}/solar-system:${{ github.sha }} \
   --env MONGO_URI=${{ env.MONGO_URI}} \
   --env MONGO_USERNAME=${{ env.MONGO_USERNAME}} \
   --env MONGO_PASSWORD=${{ env.MONGO_PASSWORD}}
set -e
echo "kubectl returned $?"
kubectl -n default get pod
sleep 10





REUSEABLE WORKFLOW TASK



Navigate to your GitHub account and use github-actions-solar-system repository within the feature/workflow branch
Use the below YAML to create a new workflow file named reusable-workflow.yml:

name: Testing - Reusable Workflow
on: 
  ??????????
jobs:
  integration-test:
    runs-on: ubuntu-latest
    steps:
      - env:
          URL: '${{ needs.dev-deploy.outputs.APP_INGRESS_URL }}'
        run: |
          curl https://$URL/live -s -k | jq -r .status | grep -i live

Do the following within reusable-workflow.yml:
1) On line 3 replace the question marks(??????) with an event trigger that can trigger this workflow by another workflow.
2) On line 11 a hardcoded string is used to get an output variable from a job that is not defined in this reusable workflow
a. This has to be replaced with an input passed to the called workflow from the caller workflow.
b. Define the inputs keyword with the following config,

Input id: ingress-url
description: Provide the Ingress URL
required: true
type: string
c. Modify line 11 to get the value from ingress-url input
3) Commit the changes


If you haven't done so already, you can fork the repository from this link: https://github.com/kodekloudhub/github-actions-solar-system





Navigate to your GitHub account and use github-actions-solar-system repository within the feature/workflow branch
Do the following within solar-system.yml workflow
1) Modify or add the integration-testing job to use the reusable workflow
2) This job should run after dev-deploy job.
3) This job should use the reusable workflow defined at this path - ./.github/workflows/reusable-workflow.yml.
a. Reusable workflow accepts an input parameter
b. Configure an input parameter with below config

key: ingress-url
value: ${{ needs.dev-deploy.outputs.APP_INGRESS_URL }}
4) Commit the changes and checkout and workflow execution

If you haven't done so already, you can fork the repository from this link: https://github.com/kodekloudhub/github-actions-solar-system
