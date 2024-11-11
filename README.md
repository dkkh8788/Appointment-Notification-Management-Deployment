# Appointment-Notification-Management-Deployment
Deployment Script Repo for Appointment-Notification-Management System


# Steps to run
 
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
        (docker pull <image-name> - pull the mages before so that container can start faster)
# Deploy services on Kubernetes using deployment files ( This will take some time)
kubectl apply -f appointment-svc-deployment.yml
kubectl apply -f notification-service-deployment.yml
# Validate Deployment
kubectl get all
# Validate APIs -
curl -X POST http://127.0.0.1:30000/book_appointment -H "Content-Type: application/json"  -d '{ "patient_id": "130", "doctor_id": 8884, "appointment_time": "2023-10-16 09:00" }'
curl http://127.0.0.1:30000/all_doctors
