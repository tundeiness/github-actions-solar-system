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
