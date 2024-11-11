# Appointment-Notification-Management-Deployment

# Deployment Plan
Minikube Cluster with a worker node where 2 Pods are deployed.

A) Appointment Management - Flask App and MongoDB Container in the same Pod

B) Notification Management Service - Flask App in the Pod and it connects to external services for sending notifications

# External Services:

AWS RDS: Database to Host Notification Templates.

AWS SES: Simple Email Service to send Email to Patients


# Steps to run Deployment
 
# Pre-requisite 
Docker Desktop is installed - https://docs.docker.com/desktop/setup/install/mac-install/

Kubectl is installed - https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/

Minikube is installed - https://minikube.sigs.k8s.io/docs/start/?arch=%2Fmacos%2Farm64%2Fstable%2Fhomebrew

# Steps:
 
# Start Mini Kube
minikube start --driver=docker --ports=30000:30000

# Validate minikube installation
minikube status

minikube service list 

# Pull Docker Images for microservices

docker pull bhavinbpalan90/notificationservice 

docker pull bits2023mt03164/appointment_management:v1

# Deploy services on Kubernetes using deployment files ( This will take some time)
kubectl apply -f appointment-svc-deployment.yml

kubectl apply -f notification-service-deployment.yml

# Validate Deployment
kubectl get all

# Setup MongoDB Container port forwarding.
kubectl port-forward pod/{$appointment-system-pod-name} 27017:27017

for example: kubectl port-forward pod/appointment-service-56d49fcf4c-tkqbs 27017:27017

# Add Sample data to mongodb.
python3 populate_data.py

# Validate APIs -
curl -X POST http://127.0.0.1:30000/book_appointment -H "Content-Type: application/json"  -d '{ "patient_id": "130", "doctor_id": 8884, "appointment_time": "2023-10-16 09:00" }'

curl http://127.0.0.1:30000/all_doctors
