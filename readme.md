# ArgoCD  

The goal of this project is to build a secure 3-tier application and deploy to AWS with resusable code (Terraform and Packer). First, we create a custom, golden ami using Packer. The image will be provisioned with all necessary dependencies. This image will be used later once we containerize our application. We'll take a Java Springboot application called PetClinic and containerize it with a Dockerfile. The image will be pushed to Dockerhub and referenced in the final stage. Finally, we use Terrform to provision all our infrastructure; custom VPC, instances, load balancers, SSL certificates, RDS, etc and run our image in it. The application should be secure in a 3-tier architecture (more details in part 3).

## Part 2

For this part of the project, we will be using Jenkins to build out a CI pipeline. The repository contains the source code of a Java Springboot application called PetClinic (details for packaging it and database are below). For the Jenkinsfile, here are the steps:

- Use Sonarcloud.io to scan for vulnerablities in src code
- Build artifact with mvn clean install and activate mysql profile
- Build Docker Image
- Push Image to DockerHub

Part 3 GitHub Repo: https://github.com/dangeo36/PART_3_Deploy_Infra_With_Terraform.git 


## Building the local artifact and accessing it from localhost 
```
git clone https://github.com/spring-projects/spring-petclinic.git
cd spring-petclinic
./mvnw package
java -jar target/*.jar
```

You can then access petclinic at http://localhost:8080/

## Database configuration

In its default configuration, Petclinic uses an in-memory database (H2) which
gets populated at startup with data. The h2 console is exposed at `http://localhost:8080/h2-console`,
and it is possible to inspect the content of the database using the `jdbc:h2:mem:testdb` URL.

The app needs to run with a profile: `spring.profiles.active=mysql` for MySQL 
You can start MySQL:

```
docker run -e MYSQL_USER=petclinic -e MYSQL_PASSWORD=petclinic -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=petclinic -p 3306:3306 mysql:8.0
```

## Tools / Dependencies
For this project, I have an Amazon EC2 instance (Ubuntu 20.04, t2.medium) that has Java 17, Jenkins, Terraform, awcli (configured with proper credentials), Maven, and Packer.
