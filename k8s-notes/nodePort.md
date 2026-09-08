A Service type that exposes your app on a static port on every node's IP address, making it reachable from outside the cluster — unlike ClusterIP, which stays internal-only.

needs- extra port mappings on kind, cluster.yml