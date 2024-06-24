Minikube installation
sudo apt install docker.io -y 
sudo systemctl unmask docker
sudo service docker restart
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
sudo dpkg -i minikube_latest_amd64.deb
rm -rf minikube_latest_amd64.deb
Follow the steps to complete the handson
Your task is to spin up three applications/pods inside a kubernetes cluster.
 A node-mongo-page-hit application that connects to a mongo db pod and an independent nginx pod.
Step - 1
•	Check whether docker & minikube are properly installed and configured. Start Minikube and execute this command to sync host docker with minikube docker eval $(minikube docker-env)
Step - 2
•	Create a folder named kubernetes-page-count in Desktop and build a docker mongo image with name fresco/node-mongo-page-hit:latest
docker pull mongo
Step - 3
•	Create a kubernetes namespace : frescons
•	apiVersion: v1
•	kind: Namespace
•	metadata:
•	  name: frescons

Step - 4
Create the following Deployments
•	name: mongo, namespace: frescons, labels: {app: mongo}, image: 'mongo:4.1', containerPort: 27017
•	name: node-mongo-page-hit, namespace: frescons, labels: {app: node-mongo-page-hit}, imagePullPolicy: Never, image: 'fresco/node-mongo-page-hit:latest', containerPort: 8000, env: [{name: PORT, value: '8000'}]
•	name: nginx, namespace: frescons, labels: {app: nginx}, image: 'nginx', containerPort: 80
•	for nginx application define a lifecylce field that would write the string Hello from Fresco Team into the file /usr/share/nginx/html/index.html on postStart
Step - 5
Create the following Services
•	name: mongo, namespace: frescons, port: 27017, targetPort: 27017
•	name: node-mongo-page-hit, namespace: frescons, port: 8000, targetPort: 8000, nodePort: 30800
•	name: nginx, namespace: frescons, port: 80, targetPort: 80, nodePort: 30080
Step - 6
Enable ingress addon in minukube. Create an Ingress router that will route the following paths to respective apps
•	name: fresco-ingress, namespace: frescons
•	path: /nginx, backend: {serviceName: nginx, servicePort: 80}
•	path: /app, backend: {serviceName: node-mongo-page-hit, servicePort: 8000}
Step - 7
•	Hit http:localhost/nginx and http:localhost/app and see if everything works fine.
•	Use curl with below commands to check.
•	curl -kL http:localhost/nginx should return Hello from Fresco Team
•	curl -kL http:localhost/app should return page hit count is: 1



