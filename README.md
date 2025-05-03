# artifactory
artifactory on AWS


# tentative diagram
<!-- ````mermaid
graph TD
    User[User / DevOps Engineer] -->|HTTPS| Ingress[ALB / NGINX Ingress Controller]
    Ingress --> Artifactory[Artifactory Pods EKS]
    Artifactory -->|JDBC| RDS[(RDS PostgreSQL)]
    Artifactory -->|Storage| PV[EBS Persistent Volumes]
    Artifactory -->|Optional| S3[(Amazon S3 - Binary Repository)]
    EKS[(Amazon EKS Cluster)] --> Artifactory
    Secrets[Secrets] --> Artifactory
    Configs[ConfigMaps / Helm values.yaml] --> Artifactory
    Monitoring[Prometheus / Grafana optional] --> Artifactory
    DNS[Route53 DNS] --> Ingress
    ACM[ACM / cert-manager TLS] --> Ingress -->


````mermaid
graph TD
    User["User / DevOps Engineer"] -->|HTTPS| Ingress["Ingress Controller (ALB)"]
    DNS["Route53 DNS"] --> Ingress
    ACM["ACM / cert-manager TLS"] --> Ingress

    subgraph "AWS"
        Ingress --> Artifactory["Artifactory Pods (EKS)"]

        subgraph "EKS Cluster"
            Secrets["Secrets (DB creds, Master key)"] --> Artifactory
            Configs["ConfigMaps / Helm values.yaml"] --> Artifactory
                subgraph "Artifactory Deployment"
                    RDS["RDS PostgreSQL"]
                    Artifactory -->|Storage| PV["EBS Persistent Volumes"]
                    Artifactory -->|JDBC| RDS
                    Artifactory -->|Optional| S3["Amazon S3 - Binary Repository"]
                end
        end
        Artifactory --> Monitoring["Prometheus / Grafana (optional)"] 


    end
