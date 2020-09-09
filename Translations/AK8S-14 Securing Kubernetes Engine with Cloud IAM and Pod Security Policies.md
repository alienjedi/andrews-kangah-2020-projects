# AK8S-14 Securing Kubernetes Engine with Cloud IAM and Pod Security Policies

## OBJECTIVES:

In this lab, you learn how to perform the following tasks:

    - Use Cloud IAM to control GKE access

    - Create and use Pod security policies to control Pod creation

    - Perform IP address and credential rotation


## IMPORTANT:

	For this lab, Qwiklabs has provisioned you with two user names available in the Connection Details dialog. In this lab we will refer to these accounts as Username 1 and Username 2.


## STEPS:

1. Use Cloud IAM roles to grant administrative access to all the GKE clusters in the project

    - Using Username 2 account which has only Viewer Role, try to create a cluster

        - Select the project

	    	gcloud config set project qwiklabs-gcp-02-bde1eb7afae9

	    - Create cluster cluster-1

	        gcloud container clusters create cluster-1 --num-nodes=3 --zone=us-central1-a

	    - Result: ERROR: (gcloud.container.clusters.create) ResponseError: code=403, message=Required "container.clusters.create" permission(s) for "projects/qwiklabs-gcp-02-bde1eb7afae9".


## Grant the GKE Admin Cloud IAM Role to Username 2

    - Using Username 1 account, Grant Admin Cloud IAM Role to Username 2

        ### Insert correct command or soo... I hate this

## Test the access of Username 2

    - Using Username 2 account, test creating clusters

        ### Insert correct command to try creating new cluster with the specified parameters

        - Result: The user does not have access to service account "default". Ask a project owner to grant you the iam.serviceAccountUser role on the service account.

## Grant the ServiceAccountUser IAM Role to Username 2

    - Using Username 1 account

    	### Insert correct command to grant Service Accounts > Service Account User to Username 2 User


2. Create and Use Pod Security Policies

    - Connect to the GKE cluster

    	- Using Username 1 account

    	    - Create environment variables for the GCP zone and cluster name that were used to create the cluster for this lab.

    	        export my_zone=us-central1-a

				export my_cluster=standard-cluster-1

		- Configure tab completion for the kubectl command-line tool.

			source <(kubectl completion bash)

		- Configure access to your cluster for kubectl:

		    gcloud container clusters get-credentials $my_cluster --zone $my_zone

			- Result: 	Fetching cluster endpoint and auth data.
						kubeconfig entry generated for standard-cluster-1.


    - Create a Pod Security Policy

        - clone the repository to the lab Cloud Shell

            git clone https://github.com/GoogleCloudPlatformTraining/training-data-analyst

        - Change to the directory that contains the sample files for this lab

            cd ~/training-data-analyst/courses/ak8s/14_IAM/

        - Create the Pod Security Policy:

            kubectl apply -f restricted-psp.yaml

        - Confirm that the Pod Security Policy has been created

        	kubectl get podsecuritypolicy restricted-psp


    - Create a ClusterRole to a Pod Security Policy

        - Grant your user account cluster-admin privileges

            kubectl create clusterrolebinding cluster-admin-binding --clusterrole cluster-admin --user student-02-7ac153dfa465@qwiklabs.net

        - Create the ClusterRole with access to the security policy

            kubectl apply -f psp-cluster-role.yaml

        - Confirm that the ClusterRole has been created

            kubectl get clusterrole restricted-pods-role


    - Create a ClusterRoleBinding for the Pod Security Policy

        - Bind the restricted-pods-role ClusterRole to the serviceaccounts group in the default namespace

            kubectl apply -f psp-cluster-role-binding.yaml

### Activate Security Policy

    - Enable the PodSecurityPolicy controller:

        gcloud beta container clusters update $my_cluster --zone $my_zone --enable-pod-security-policy

   
## Test the Pod Security Policy

    - Attempt to run the privileged Pod

    	Insert correct command


    - Edit the privileged-pod.yaml file and remove the two lines at the bottom that invoke the privileged container security context

        sudo nano privileged-pod.yaml

    - Deploy the privileged Pod

        kubectl apply -f privileged-pod.yaml