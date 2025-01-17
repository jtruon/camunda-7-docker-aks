# Camunda 7 Deployment on AKS

This README provides step-by-step instructions to deploy Camunda 7 on an AKS (Azure Kubernetes Service) cluster.

## Prerequisites

1. An AKS cluster must be spun up and configured.
2. `kubectl` CLI should be installed and connected to your AKS cluster.

## Steps to Deploy Camunda 7

### 1. Deploy PostgreSQL

1. Apply the PostgreSQL deployment manifest:
   ```bash
   kubectl apply -f deployPostgres.yml
   ```
2. Verify that the PostgreSQL pod is running:
   ```bash
   kubectl get pods -n postgres
   ```
3. Connect to the PostgreSQL instance using the credentials provided in the `deployPostgres.yml` file. Confirm the connection:
   ```bash
   psql -h <POSTGRES_HOST> -U <USERNAME> -d <DATABASE_NAME>
   ```

### 2. Deploy Camunda

1. Apply the Camunda deployment manifest:
   ```bash
   kubectl apply -f deployment.yml
   ```
2. Verify the Camunda pod is running:
   ```bash
   kubectl get pods -n camunda
   ```

### 3. Adjust Configuration (Optional)

If you need to modify the Camunda configuration:

1. Retrieve the running pod details:
   ```bash
   kubectl get pods -n camunda
   ```
   Example output:
   ```
   NAME                         READY   STATUS    RESTARTS   AGE
   camunda-84d5f5c9d7-mh96l    1/1     Running   0          5m
   ```

2. Copy the configuration file from the pod:
   ```bash
   kubectl cp camunda-84d5f5c9d7-mh96l:/home/camunda/conf/bpm-platform.xml ./bpm-platform.xml -n camunda
   ```

3. Edit the configuration file locally as needed:
   ```bash
   nano bpm-platform.xml
   ```

4. Apply the updated configuration back to the pod (if necessary, restart the pod to reflect changes):
   ```bash
   kubectl cp ./bpm-platform.xml camunda-84d5f5c9d7-mh96l:/home/camunda/conf/bpm-platform.xml -n camunda
   ```

## Notes
- Ensure the namespace in your `kubectl` commands matches the namespace specified in the deployment manifests (e.g., `postgres`, `camunda`).
- For troubleshooting, review pod logs:
  ```bash
  kubectl logs camunda-84d5f5c9d7-mh96l -n camunda
  ```
- Use a Kubernetes monitoring tool like `kubectl get all -n camunda` to view the overall state of your deployment.

## Conclusion
Following these steps will deploy Camunda 7 and PostgreSQL on AKS successfully. Modify configurations as needed to suit your use case.

