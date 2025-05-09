# DEVELOPMENT Stage Building Artifact
#----------------------------------

# Import docker image with maven installed
FROM maven:3.8.3-openjdk-17 as builder 

# Set working directory
WORKDIR /src

# Copy source code from local to container
COPY . /src

# Build application and skip test cases
RUN mvn clean install -DskipTests=true

# PRODUCTION Stage  
#----------------------------------
# Import small size java image
FROM openjdk:17-alpine as deployer

WORKDIR /src

# Copy build from stage 1 (builder)
COPY --from=builder /src/target/*.jar /src/target/bankapp.jar

# Copy the import.sql file into the Docker image (you may need to place this in `src/main/resources`)
COPY src/main/resources/import.sql /docker-entrypoint-initdb.d/

# Expose application port 
EXPOSE 8080

# Start the application
ENTRYPOINT ["java", "-jar", "/src/target/bankapp.jar"]
