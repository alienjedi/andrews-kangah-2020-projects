# App Dev: Setting up a Development Environment v1.1

## OBJECTIVES:

In this lab, you learn how to perform the following tasks:

    - Provision a Google Compute Engine instance.

    - Connect to the instance using SSH.

    - Install software on the instance.

    - Verify the software installation.


## STEPS:

1. Creating a Compute Engine Virtual Machine Instance

    - Provision a new Google Compute Engine virtual machine instance.

        gcloud beta compute --project=qwiklabs-gcp-04-2dbc461adb30 instances create dev-instance --zone=us-central1-a --machine-type=e2-medium --subnet=default --network-tier=PREMIUM --maintenance-policy=MIGRATE --service-account=990971886699-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/cloud-platform --tags=http-server --image=debian-10-buster-v20200902 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=dev-instance --no-shielded-secure-boot --no-shielded-vtpm --no-shielded-integrity-monitoring --reservation-affinity=any

        gcloud compute --project=qwiklabs-gcp-04-2dbc461adb30 firewall-rules create default-allow-http --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server



2. Install software on the VM instance

    - Update the Debian package list

        sudo apt-get update

    - Install Git

        sudo apt-get install git

    - Download the Node.js setup script

        curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -

    - Install npm and Node.js



3. Configure the VM to Run Application Software


    - Check the version of Node.js

        node -v

    - Clone the class repository

        git clone https://github.com/GoogleCloudPlatform/training-data-analyst

    - Change the working directory

        cd ~/training-data-analyst/courses/developingapps/nodejs/devenv/

    - Run a simple web server

        sudo node server/app.js

    - Install the Node.js library for Compute Engine

        npm install

    - Run a simple Node.js application that lists Compute Engine instances

        node list-gce-instances.js

