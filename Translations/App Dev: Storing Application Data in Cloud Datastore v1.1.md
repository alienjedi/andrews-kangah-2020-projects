# App Dev: Storing Application Data in Cloud Datastore v1.1


## OBJECTIVES

In this lab, you will learn how to perform the following tasks:

   - Harness Cloud Shell as your development environment.

  - Integrate Cloud Datastore into a NodeJS application.


## STEPS


1. Configure and run the case study application

    - Clone the repository for the class

        git clone https://github.com/GoogleCloudPlatform/training-data-analyst

    - Change the working directory

        cd ~/training-data-analyst/courses/developingapps/nodejs/datastore/start

    - Export an environment variable, GCLOUD_PROJECT that references the GCP Project ID

        export GCLOUD_PROJECT=$DEVSHELL_PROJECT_ID

    - Install the application dependencies

    	npm install

   	-  Run the application

   		npm start



2. Adding Entities to Cloud Datastore

    - Create an App Engine application to provision Cloud Datastore

        gcloud app create --region "us-central"

    - Change directory to Datastore module datastore.js

        ~/training-data-analyst/courses/developingapps/nodejs/datastore/start/server/gcp

    - Write code filling TO-DO gaps in order to save questions to Cloud Datastore

        - Open datastore.js

        	sudo nano datastore.js

        - Load the config module from the parent folder.

            const config = require('../config');

        - Load the @google-cloud/datastore module.

            const Datastore = require('@google-cloud/datastore');

        - Declare a Datastore client object named ds.

            const ds = Datastore({
			 projectId: config.get('GCLOUD_PROJECT')
			});

	    - Declare a constant named kind, initialized with the value 'Question'.

	        const kind = 'Question';

	    - In the create(...) function, remove the existing Promise.resolve({}) placeholder statement from the create(...) function.

	        function create({ quiz, author, title, answer1, answer2,
                  answer3, answer4, correctAnswer }) {

        - Declare a constant called key to store the key for this entity.

            const key = ds.key(kind);

        - Declare a constant named entity and initialize it with the key and the quiz question properties extracted from the form data.

            const entity = {
				   key,
				// The entity's members are represented in a data property.
				// This is an array where each element represents one
				// member in the entity. Each element is an object with a // name and a value
				   data: [
				     { name: 'quiz', value: quiz },
				     { name: 'author', value: author },
				     { name: 'title', value: title },
				     { name: 'answer1', value: answer1 },
				     { name: 'answer2', value: answer2 },
				     { name: 'answer3', value: answer3 },
				     { name: 'answer4', value: answer4 },
				     { name: 'correctAnswer', value: correctAnswer },
				   ]
				 };

		- Use the Datastore client object (ds) to save the entity by calling the save(entity) method.

		    return ds.save(entity);

    - Run the application and create a Cloud Datastore entity

        npm start
