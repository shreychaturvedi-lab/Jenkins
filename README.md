Angular frontend and Springboot microservice using Docker Compose
======================================================================

1. Checkout docker-compose.yml, nginx.conf, Dockerfile, target and recipe folders

2. Create a Dockerfile for creating a docker image from the Spring Boot Application FROM openjdk:8 VOLUME /tmp ADD target/restful-web-services-0.0.1-SNAPSHOT.jar restful-web-services-0.0.1-SNAPSHOT.jar EXPOSE 8080 ENTRYPOINT ["java", "-jar", "springboot-docker-compose.jar"]

3. Create docker-compose.yml `version: '3.5' services: springapi: image: springboot-docker-compose-app:1 build: context: ./ dockerfile: Dockerfile container_name: springapi volumes: - ./data/springboot-docker-compose-app ports: - "8080:8080" networks: - backendNetwork - frontendNetwork angular-service: image: nginx:alpine container_name: angular ports: - 4200:80 depends_on: - springapi ports: - "4200:80" volumes: - ./nginx.conf:/etc/nginx/nginx.conf - ./recipe/dist/recipe/:/usr/share/nginx/html networks: - frontendNetwork

4. Create the Docker image - springboot-docker-compose and nginx:alpine start application using docker compose created in #2. docker-compose up

5. To access the application, use Username - Dipendi, Password - Dipendi 

6. Stop docker container docker-compose down



Basic Understanding of how flow works
======================================================================

Backend Application:
--------------------

1. "springboot/restful-web-services" folder contains backend code written in java springboot using REST Api to CRUD operations 
    create @POST
    Retrieve @GET
    Update @PUT
    Delete @DELETE
    Retrieve by id @GET/{id} (for retrieving specific recipe details)
    
2. There is a use of JPA repository and H2 embedded database. 
    - data.sql file is attahed already. That's why when you launch the application, you will see the data is already present.
    - acsess H2 database using http://localhost:8080/h2-console and jdbc url - 'jdbc:h2:mem:testdb'
    
3. There are testcases written using Mockito Junit framework

4. "target" folder contains jar file for deploying java applciation
    
    
Frontend Application:
---------------------

1. "angular" folder contains the code for fronetend application wriitent using angular framework

2. This application has many components:
    - Welcome - should display all recipes submitted by all users
    - Login - this page allows user to access the module My Recipe 
    - My Recipes - after success login to application, user can access this page. Here, user can add recipe, update and delete existing recipe
    - logout - displays for only logged in users and allow user to logout from the application 
    - Header - contains menu items e.g. Login, Welcome, My Recipes, Logout
    
3. There are three services present:
    - Authentication service for user authentication
    - RouteGuard Service to restrict user's access to particular modules e.g. My Recipes module can only be displayed once user is logged in
    - RecipeDataService to connect frontend with jpa resources 
    
4. "recipe" folder contains the code for deploying angular application 
