# Getting Started with App Engine

## OBJECTIVES:

In this lab, you learn how to perform the following tasks:

   - Initialize App Engine.

   - Preview an App Engine application running locally in Cloud Shell.

   - Deploy an App Engine application, so that others can reach it.

   - Disable an App Engine application, when you no longer want it to be visible.


## STEPS:



1. Initialize App Engine


    - Initialize your App Engine app with your project and choose its region:

        gcloud app create --project=$DEVSHELL_PROJECT_ID --region=us-central

    - Clone the source code repository for a sample application in the hello_world directory:

        git clone https://github.com/GoogleCloudPlatform/python-docs-samples

    - Navigate to the source directory:

        cd python-docs-samples/appengine/standard_python3/hello_world





2. Run Hello World application locally

    
    - Download and update the packages list.

        sudo apt-get update

    - Set up a virtual environment in which you will run your application. Python virtual environments are used to isolate package installations from the system

        sudo apt-get install virtualenv

    - Activate the virtual environment.

        virtualenv -p python3 venv

        source venv/bin/activate

    - Navigate to your project directory and install dependencies.

        pip install  -r requirements.txt


    - Run the application:

        python main.py




3. Deploy and run Hello World on App Engine

    - Navigate to the source directory:

        cd ~/python-docs-samples/appengine/standard_python3/hello_world

    - Deploy your Hello World application:

    	gcloud app deploy

    - View the app at http://qwiklabs-gcp-02-169a7ce124b5.appspot.com

        gcloud app browse
