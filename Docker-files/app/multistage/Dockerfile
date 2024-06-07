# First Stage: Build Image
FROM openjdk:11 AS BUILD_IMAGE
WORKDIR /app

# Update package list and install Maven
RUN apt-get update 
RUN apt-get install -y maven

# Clone the repository
ADD https://github.com/devopshydclub/vprofile-project.git .
RUN ls

# Change directory, checkout the 'docker' branch, and build the project
RUN cd vprofile-project && git checkout docker && mvn install

# Second Stage: Runtime Image
FROM tomcat:9-jre11

# Remove default web apps
RUN rm -rf /usr/local/tomcat/webapps/*

# Copy the built WAR file from the build stage
COPY --from=BUILD_IMAGE /vprofile-project/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

# Expose port 8080
EXPOSE 8080

# Run Tomcat in the foreground
CMD ["catalina.sh", "run"]
