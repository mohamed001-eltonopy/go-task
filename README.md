# golang-task
# create a Docker image
docker build -t tonopy/go-app:latest .
# log in to Docker Hub 
docker login
# push your new image to Docker Hub 
docker push -t tonopy/go-app:latest 
# Deploy this microservice in specific ns
kubectl create ns microservice
kubectl apply -f deployment.yaml -n microservices
# Create Helmchart for each microservice
helm create microservice
# you will see folder "microservice" is created which is our chart and contain auto-generated files inside
# in folder "microservice" helm chart
# delete these files (tests filder , _helpers.tpl , hpa.yaml , NOTES.txt , serviceaccount.yaml)
#  delete the content inside these files:  (deployment.yaml , service.yaml , values.yaml)
# Check that you provide valid kubernetes yaml files 
helm install --dry-run -f my-app-values.yaml my-app microservice/
# Deploy Microservices"my-app" using Helm into k8s cluster
helm install  -f my-app-values.yaml my-app microservice/
# you can deploy helm cart using "helmfile"
#  Install Helm file "helmfile.yaml" ( https://github.com/roboll/helmfile )
# you can run helm file as a container
sudo docker run --rm --net=host -v "${HOME}/.kube:/root/.kube" -v "${HOME}/.config/helm:/root/.config/helm" -v "${PWD}:/wd" --workdir /wd quay.io/roboll/helmfile:helm3-v0.135.0 helmfile sync 



