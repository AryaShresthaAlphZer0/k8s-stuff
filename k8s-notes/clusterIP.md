md about clusterip

ClusterIP
The default Kubernetes Service type. Gives a set of Pods a single, stable, internal IP + DNS name that other things inside the cluster can use to reach them — not accessible from outside the cluster.

Why it exists
Pods are ephemeral — they restart and get new IPs constantly. A ClusterIP Service gives you one fixed internal address that automatically load-balances across whichever Pods currently match its selector, regardless of how many times those Pods get replaced.

kubectl port-forward service/nginx-service 8080:80