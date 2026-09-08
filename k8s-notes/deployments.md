just give it in chat

Kubernetes Deployments
A Deployment is a Kubernetes object that manages a set of identical, replicated Pods, and handles rolling out changes to them safely.

What it does
Ensures a specified number of Pod replicas are running at all times
Automatically replaces Pods that crash, get deleted, or fail health checks
Handles rolling updates when you change the Pod template (e.g. new image version) — replacing old Pods with new ones gradually, with zero downtime
Supports rollbacks to a previous version if an update goes wrong
Can be scaled up or down on demand

kubectl apply -f deployment.yaml          # create/update
kubectl get deployments                   # list deployments
kubectl get pods                          # list pods
kubectl describe deployment <name>        # detailed info + events
kubectl scale deployment <name> --replicas=5   # scale up/down
kubectl rollout status deployment <name>  # watch a rollout
kubectl rollout undo deployment <name>    # rollback to previous version
kubectl delete pod <pod-name>             # delete one pod (auto-replaced)
kubectl delete -f deployment.yaml         # delete the whole deployment