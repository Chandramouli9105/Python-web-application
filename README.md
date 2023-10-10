# Python-web-application
Building a simple Python web application

=> The application uses the Flask framework and maintains a hit counter in Redis.

Prerequisites
=> Install Docker Engine and Docker Compose

Step 1: Define the application dependencies

	    1. Create a directory for the project:
			# mkdir web-app
			# cd web-app
			
		  2. Create a file called app.py in your project directory and paste the following code in:
			-> in app.py redis is the hostname of the redis container on the application's network. We use the default port for Redis, 6379
			
		  3. Create another file called requirements.txt in your project directory and 
		     paste the following code in:
		
			  flask
			  redis
			
Step 2: Create a Dockerfile
  		-> Create a file named Dockerfile

Step 3: Define services in a Compose file
		  -> Create a file called compose.yaml
		
    	This Compose file defines two services: web and Redis.
    	The web service uses an image that's built from the Dockerfile in the current directory. It then binds the container and the host machine to the exposed port, 8000. 
      This example service uses the default port for the Flask web server, 5000.
	
 Step 4: Build and run your app with Compose

		start up your application by running 
		# docker compose up
				
		Check using public_url:8000
		
Step 5: Edit the Compose file to add a bind mount

          services:
            web:
              build: .
              ports:
                - "8000:5000"
              volumes:
                - .:/code
              environment:
                FLASK_DEBUG: "true"
            redis:
              image: "redis:alpine"
	
	
The new volumes key mounts the project directory (current directory) on the host to /code inside the container, allowing you to modify the code on the fly, without having to rebuild the image. The environment key sets the FLASK_DEBUG environment variable, which tells flask run to run in development mode and reload the code on change. This mode should only be used in development.
	
Step 6: Re-build and run the app with Compose
    		# docker compose up
        or 
    		# docker compose up -d   -> run in detached mode
    		# docker compose ps      -> see what currently running
    		#docker compose stop    -> to stop
		
Step 7: Update the application
		    change the Hello World! message to Hello from Docker! in app.py
		
	return 'Hello from Docker! I have been seen {} times.\n'.format(count)

Refresh the app in your browser. The greeting should be updated, and the counter should still be incrementing.

