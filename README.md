# Kubernetes Basics in DevOps Automation and Scaling

## Scenario

I was tasked with deploying a microservices application on Kubernetes. My goal was to ensure the application could dynamically scale based on traffic and that traffic would be balanced across instances for optimal performance.

## Deliverables

1. **Comparison Analysis**: A brief comparison between Kubernetes and Docker Swarm.
2. **Deployment Documentation**: Step-by-step guide to deploying a basic application on Kubernetes.
3. **Scaling and Load Balancing Documentation**: A guide demonstrating Kubernetes scaling and load balancing setup.

---

## Project Tasks

### Task 1: Compare Kubernetes with Other Orchestration Tools

**Objective**: I needed to understand why Kubernetes is widely adopted for orchestrating applications in DevOps by comparing it with Docker Swarm.

#### Steps:

1. **Research Kubernetes and Docker Swarm**:

   - **Kubernetes**: Open-source, highly scalable, and feature-rich orchestration platform.
   - **Docker Swarm**: Simpler and built into Docker, but less feature-rich compared to Kubernetes.

2. **Compare Key Aspects**:

   - **Scalability**: Kubernetes supports thousands of nodes, while Docker Swarm is limited to smaller clusters.
   - **Automation**: Kubernetes offers advanced automation features like self-healing, rolling updates, and autoscaling.
   - **Community Support**: Kubernetes has a larger, more active community.
   - **DevOps Integration**: Kubernetes integrates seamlessly with CI/CD pipelines and DevOps tools.

**Deliverable**: I wrote a one-page report comparing Kubernetes and Docker Swarm, concluding why Kubernetes is preferred for DevOps.

---

### Task 2: Deploy a Basic Application on Kubernetes

**Objective**: My goal was to deploy a containerized application using Kubernetes, exploring fundamental components like Pods, Deployments, and Services.

#### Steps:

1. **Set Up a Kubernetes Cluster**:
![Screenshot (123)](https://github.com/user-attachments/assets/bf252de1-0431-4177-844b-93c4e46332fb)

   ```bash
   minikube start
   ```

2. **Create a Namespace**:
![Screenshot (124)](https://github.com/user-attachments/assets/6df8005f-8548-4e8f-a147-5970ecb3a733)

   ```bash
   kubectl create namespace my-app
   ```

3. **Define a Deployment**: Created a `deployment.yaml` file.
![Screenshot (125)](https://github.com/user-attachments/assets/ca643455-e0f5-496e-a348-db56e19dc75f)

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: my-app
     namespace: my-app
   spec:
     replicas: 3
     selector:
       matchLabels:
         app: my-app
     template:
       metadata:
         labels:
           app: my-app
       spec:
         containers:
         - name: my-app
           image: nginx:latest
           ports:
           - containerPort: 80
   ```

4. **Apply the Deployment**:
![Screenshot (127)](https://github.com/user-attachments/assets/9b8fc725-de11-40cc-81f0-46506f6d05c3)

   ```bash
   kubectl apply -f deployment.yaml
   ```

5. **Expose the Application**: Created a `service.yaml` file.

   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: my-app-service
     namespace: my-app
   spec:
     selector:
       app: my-app
     ports:
     - protocol: TCP
       port: 80
       targetPort: 80
     type: LoadBalancer
   ```

6. **Apply the Service**:
![Screenshot (132)](https://github.com/user-attachments/assets/43aea336-bc39-4724-8461-43ff56553327)
![Screenshot (133)](https://github.com/user-attachments/assets/73713d2a-d792-4432-8bb3-94176059eddb)

   ```bash
   kubectl apply -f service.yaml
   ```

7. **Verify the Deployment**:
![Screenshot (134)](https://github.com/user-attachments/assets/4fef17da-3c7c-4aa4-ab0b-f6bee1e0905e)

   ```bash
   kubectl get deployments -n my-app
   kubectl get pods -n my-app
   kubectl get svc -n my-app
   ```

---

### Task 3: Implement Scaling and Load Balancing

**Objective**: I set up dynamic scaling for the application and configured load balancing to distribute traffic evenly.

#### Steps:

1. **Manual Scaling**:
![Screenshot (135)](https://github.com/user-attachments/assets/2f6e249d-b03a-48f0-bdc0-7bcb80ef739a)

   ```bash
   kubectl scale deployment my-app --replicas=5 -n my-app
   ```

2. **Autoscaling**:
![Screenshot (136)](https://github.com/user-attachments/assets/3885a702-4206-49b1-aa38-814afa683a90)

   ```bash
   kubectl autoscale deployment my-app --cpu-percent=50 --min=3 --max=10 -n my-app
   ```

3. **Load Balancing**:

   ```bash
   minikube service my-app-service -n my-app
   ```

4. **Simulate Traffic**:
![Screenshot (137)](https://github.com/user-attachments/assets/6dcd5c8f-e190-4197-9b52-b4b25a5d5047)

   ```bash
   ab -n 1000 -c 10 http://192.168.49.2:32517/
   ```

5. **Observe Scaling**:
![Screenshot (138)](https://github.com/user-attachments/assets/10125ecc-e1c2-4d10-9daa-572b02d0d684)

   ```bash
   kubectl get hpa -n my-app
   kubectl get pods -n my-app
   ```

---

## Testing and Validation

1. **Test**: I simulated varying levels of traffic using `ab` or `curl` and observed how the application scaled up and down.
2. **Validate**: I checked that the application scaled as expected and verified that traffic was balanced across all instances.

---

## Documentation and Reflection

**Documentation**:
I documented each part of the project, including:

- Comparison analysis.
- Deployment steps.
- Scaling and load balancing setup.

**Reflection**:
I reflected on how Kubernetes simplifies managing large-scale applications and discussed how it improves reliability and scalability in a DevOps environment.

---

## Conclusion

This lab guide provided me with a hands-on approach to learning Kubernetes basics, focusing on deployment, scaling, and load balancing. By completing this project, I gained practical experience with Kubernetes and a deeper understanding of its role in DevOps automation and scaling.

---

## Code Snippets and Commands Summary

### Deployment

```bash
kubectl create namespace my-app
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl get deployments -n my-app
kubectl get pods -n my-app
kubectl get svc -n my-app
```

### Scaling

```bash
kubectl scale deployment my-app --replicas=5 -n my-app
kubectl autoscale deployment my-app --cpu-percent=50 --min=3 --max=10 -n my-app
```

### Load Balancing

```bash
minikube service my-app-service -n my-app
ab -n 1000 -c 10 http://192.168.49.2:32517/
```

