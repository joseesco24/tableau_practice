# ---------------------------------------------------------------------------------------------------------------------
# ** stage 1: production image
# ---------------------------------------------------------------------------------------------------------------------

# ** info: declaration of the image base version
FROM quay.io/app-sre/ubi8-ubi

# ** info: updating image packages
RUN yum -y update

# ** info: installing the wget package
RUN yum install wget -y

# ** info: downloading the tableau bridges files
RUN wget https://downloads.tableau.com/tssoftware/TableauBridge-20233.24.0119.1557.x86_64.rpm
RUN wget https://jdbc.postgresql.org/download/postgresql-42.7.0.jar

# ** info: installing tableau bridge
RUN ACCEPT_EULA=y yum -y install TableauBridge-20233.24.0119.1557.x86_64.rpm

# ** info: creating the tableau bridge jdbc directory
RUN mkdir -p /opt/tableau/tableau_bridge/jdbc

# ** info: moving the postgresql jdbc file to the tableau bridge jdbc directory
RUN mv postgresql-42.7.0.jar /opt/tableau/tableau_bridge/jdbc

# ** info: removing the tableau bridge rpm file
RUN rm -f TableauBridge-20233.24.0119.1557.x86_64.rpm

# ** info: setting environment variables
ARG LANGUAGE="en_US.utf8"
ARG LC_ALL="en_US.utf8"
ARG LANG="en_US.utf8"

# ** info: cpoying the tableau bridge token file to the working directory
COPY ["token", "token/"]

# ** info: setting the token file permissions
RUN chmod 600 /token/testToken.txt

# ** info: setting the tableau bridge executbale as the entrypoint
ENTRYPOINT ["/opt/tableau/tableau_bridge/bin/TabBridgeClientWorker", "-e"]

# ** info: setting the tableau bridge entrypoint arguments
CMD ["--userEmail=josedanielescobarmurci@gmail.com", "--patTokenId=testToken", "--client=bridgeTestClient", "--site=testjose", "--patTokenFile=/token/testToken.txt"]