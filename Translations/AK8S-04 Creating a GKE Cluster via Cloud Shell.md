# LAB: AK8S-04 Creating a GKE Cluster via Cloud Shell

## OBJECTIVES: 

In this lab, you learn how to perform the following tasks:

   - Use kubectl to build and manipulate GKE clusters

   - Use kubectl and configuration files to deploy Pods

   - Use Container Registry to store and deploy containers



## STEPS:

1. Deploy GKE clusters

    - Set the environment variable for the zone and cluster name

        export my_zone=us-central1-a

	    export my_cluster=standard-cluster-1

	- Create a Kubernetes cluster

	    gcloud container clusters create $my_cluster --num-nodes 3 --zone $my_zone --enable-ip-alias


2. Modify GKE clusters

    -  Modify standard-cluster-1 to have four nodes:

        gcloud container clusters resize $my_cluster --zone $my_zone --num-nodes=4
        


3. Connect to a GKE cluster

### Use Cloud Shell to authenticate to a GKE cluster and then inspect the kubectl configuration files.

   - Create a kubeconfig file with the credentials of the current user (to allow authentication) and provide the endpoint details for a specific cluster (to allow communicating with that cluster through the kubectl command-line tool)

        gcloud container clusters get-credentials $my_cluster --zone $my_zone


   - Open the kubeconfig file with the nano text editor to view its content:

        nano ~/.kube/config



4. Use kubectl to inspect a GKE cluster

    - Execute the following command to print out the content of the kubeconfig file in cloud shell:

        kubectl config view

    - Execute the following command to print out the cluster information for the active context:

        kubectl cluster-info

    - Execute the following command to print out the active context:

        kubectl config current-context

    - Execute the following command to print out some details for all the cluster contexts in the kubeconfig file:

        kubectl config get-contexts

    - Execute the following command to change the active context:

        kubectl config use-context gke_${GOOGLE_CLOUD_PROJECT}_us-central1-a_standard-cluster-1

        - Result: Switched to context "gke_qwiklabs-gcp-01-d8e335679940_us-central1-a_standard-cluster-1".

    - View the resource usage across the nodes of the cluster:

        kubectl top nodes

    - Enable bash autocompletion for kubectl:

        source <(kubectl completion bash)

    - Type kubectl and press the Tab key twice to show suggestions:

        - Result: 
	        alpha          apply          certificate    convert        delete         edit           get            options        proxy          scale          uncordon
			annotate       attach         cluster-info   cordon         describe       exec           kustomize      patch          replace        set            version
			api-resources  auth           completion     cp             diff           explain        label          plugin         rollout        taint          wait
			api-versions   autoscale      config         create         drain          expose         logs           port-forward   run            top



5. Deploy Pods to GKE clusters

### Use kubectl to deploy Pods to GKE


   - Deploy the latest version of nginx as a Pod named nginx-1:

       kubectl run nginx-1 --image nginx:latest

       - Result: pod/nginx-1 created


   - View all the deployed Pods in the active context cluster:

       kubectl get pods


   - Set environment variable for Pod name to avoid possible spelling/typing errors

       export my_nginx_pod=nginx-1


   - Make sure environment variable is set

       echo $my_nginx_pod

       - Result: nginx-1


   - View the complete details of the Pod you just created.

       kubectl describe pod $my_nginx_pod


### Push a file into a container


   - Open a file named test.html in the nano text editor.

       nano ~/test.html


   - Add the following text (shell script) to the empty test.html file:

        <html> <header><title>This is title</title></header>
			<body> Hello world </body>
		</html>


   - Place the file into the appropriate location within the nginx container in the nginx Pod to be served statically

       kubectl cp ~/test.html $my_nginx_pod:/usr/share/nginx/html/test.html

    
### Expose the Pod for testing

    
   - Create a service to expose our nginx Pod externally:

       kubectl expose pod $my_nginx_pod --port 80 --type LoadBalancer

       - Result:   0 --type LoadBalancer 
                   service/nginx-1 exposed


   - View details about services in the cluster:

       kubectl get services


   - Verify that the nginx container is serving the static HTML file that you copied using the External IP from the LoadBalancer.

       curl http://34.123.237.132/test.html

   - View the resources being used by the nginx Pod:

       kubectl top pods



6. Introspect GKE Pods

### Prepare the environment


   - Clone the repository to the lab Cloud Shell:

       git clone https://github.com/GoogleCloudPlatformTraining/training-data-analyst


   - Change to the directory that contains the sample files for this lab:

       cd ~/training-data-analyst/courses/ak8s/04_GKE_Shell/


   - Deploy the manifest:

       kubectl apply -f ./new-nginx-pod.yaml


### Use shell redirection to connect to a Pod


   - Start an interactive bash shell in the nginx container:

       kubectl exec -it new-nginx /bin/bash

       - Result: root@new-nginx:/#

   - Install the nano text editor (the pod has no editor installed):
        
       apt-get update
       apt-get install nano


   - Switch to the static files directory and create a test.html file:

       cd /usr/share/nginx/html
       nano test.html

       - Type in the code:

	    <html> <header><title>This is title</title></header>
			<body> Hello world </body>
		</html>

       - Exit from the pod (root account):

	    exit


       - Set up port forwarding from Cloud Shell to the nginx Pod (from port 10081 of the Cloud Shell VM to port 80 of the nginx container):

	   kubectl port-forward new-nginx 10081:80

	   - Result: 
		    Forwarding from 127.0.0.1:10081 -> 80
	        	Forwarding from [::1]:10081 -> 80


       - Test the modified nginx container through the port forwarding:

	    curl http://127.0.0.1:10081/test.html


### View the logs of a Pod


   - Display the logs and stream new logs as they arrive (and also include timestamps) for the new-nginx Pod in another cloud shell window:

       kubectl logs new-nginx -f --timestamps

