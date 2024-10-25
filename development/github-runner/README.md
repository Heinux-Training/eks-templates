## Create a self-hosted runners for GitHub Actions in EKS

- Install Action Controller with helm command 
``` bash
helm repo add actions-runner-controller https://actions-runner-controller.github.io/actions-runner-controller

helm repo update

helm upgrade --install --namespace actions-runner-system \
  --create-namespace --wait actions-runner-controller \
  actions-runner-controller/actions-runner-controller \
  --set syncPeriod=1m
```

- Install a runner Deployment for Repository, Organization or Enterprise
``` yaml
apiVersion: actions.summerwind.dev/v1alpha1
kind: RunnerDeployment
metadata:
  name: k8s-action-runner
  namespace: actions-runner-system
spec:
  replicas: 1
  template:
    spec:
      organization: Heinux-Training
      labels:
        - "demo-app"
        - "heinux"
        - "self-hosted"
      group: "Development"
```

- Apply the Runner Deployment 
``` bash
k apply -f github-runner/demo-runner.yml
```

### Reference

Reference Blog link - https://devopscube.com/github-actions-runner-aws-eks/