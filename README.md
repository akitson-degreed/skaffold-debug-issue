# ASPNET sample


## Local Dev Environment Setup
The local development stack described below uses will build and deploy the application to a local kubernetes cluster (minikube).  In addition, it will "watch" files for changes, and automatically rebuild and deploy when changes are detected.

Any shell commands are best executed using an admin shell.  If your environment is windows, powershell run as an administrator is best.

### Install Tools
- Docker Desktop: https://www.docker.com/get-started
   - If prompted, don't enable "Kubernetes in Docker"
- Kubectl: https://kubernetes.io/docs/tasks/tools/
    - Version 1.22.x
- Minikube: https://minikube.sigs.k8s.io/docs/start/
    - Version 1.23.x
- Skaffold: https://skaffold.dev/docs/install/
    - Version 1.35.1
- Pack CLI: https://buildpacks.io/docs/tools/pack/
    - Version 0.22.0
- Helm: https://helm.sh/docs/intro/install/
    - Version 3.5.4


### Setup Minikube
```
minikube start
minikube addons enable ingress
```
_Above commands must be repeated if you ever reset your minikube instance with `minikube delete`._


### Setup Skaffold
```
skaffold config set --global local-cluster true
```

### Build & Run
Change to directory with the code in it, and execute the command `skaffold dev`.

Once the deployment is complete, the application can be viewed at [http://localhost:5000](http://localhost:5000).

By default, this process "watches" for changes to files, and rebuilds & redeploys the app automatically when changes are detected.

## Troubleshooting

### Problems running in local dev env
Browse to edge://net-internals/#hsts (Edge) or chrome://net-internals/#hsts (Chrome) and scroll down to "Delete domain security policies".  Enter "localhost" in the input and press "Delete"


### Viewing Application Logs
To view the logs of the running application, open a powershell window and execute the following commands.
```
kubectl get pods
.
.
#output similar to below
NAME                                          READY   STATUS    RESTARTS   AGE
visier-utility-release-web-848c8754f7-wmght   1/1     Running   0          7m33s
```
Make note of the name of the pod in the result above for use in the subsequent command.
```
kubectl logs {pod-name}
# example from name above:
# kubectl logs visier-utility-release-web-848c8754f7-wmght
.
.
.
# output similar to below
warn: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[35]
      No XML encryptor configured. Key {9fcb3d04-2221-409a-8fc4-1229f84e86e6} may be persisted to storage in unencrypted form.
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://0.0.0.0:8080
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Production
info: Microsoft.Hosting.Lifetime[0]
      Content root path: /workspace
```

### Getting a shell to the running container
```
kubectl exec --stdin --tty pod/{pod-name} -- bash
```
References:
- [https://kubernetes.io/docs/tasks/debug-application-cluster/get-shell-running-container/](https://kubernetes.io/docs/tasks/debug-application-cluster/get-shell-running-container/)


### Problems running in local dev env
- Browse to edge://net-internals/#hsts (Edge) or chrome://net-internals/#hsts (Chrome) and scroll down to "Delete domain security policies".  Enter "localhost" in the input and press "Delete"

