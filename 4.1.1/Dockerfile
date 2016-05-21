# GlassFish on Docker
FROM glassfish/openjdk 

# Maintainer
MAINTAINER Bruno Borges <bruno.borges@oracle.com>

# Set Locale and Language
ENV LANG en_US.UTF-8  
ENV LANGUAGE en_US:en  
ENV LC_ALL en_US.UTF-8

# Set environment variables and default password for user 'admin'
ENV GLASSFISH_PKG=glassfish-4.1.1.zip \
    GLASSFISH_URL=http://download.oracle.com/glassfish/4.1.1/release/glassfish-4.1.1.zip \
    GLASSFISH_HOME=/glassfish4 \
    PATH=$PATH:/glassfish4/bin \
    PASSWORD=glassfish

# Install packages, download and extract GlassFish
# Setup password file
# Enable DAS
RUN apk add --update wget bash unzip && \
    wget --no-check-certificate $GLASSFISH_URL && \
    unzip -o $GLASSFISH_PKG && \
    rm -f $GLASSFISH_PKG && \
    apk del wget unzip && \
    echo "--- Setup the password file ---" && \
    echo "AS_ADMIN_PASSWORD=" > /tmp/glassfishpwd && \
    echo "AS_ADMIN_NEWPASSWORD=${PASSWORD}" >> /tmp/glassfishpwd  && \
    echo "--- Enable DAS, change admin password, and secure admin access ---" && \
    asadmin --user=admin --passwordfile=/tmp/glassfishpwd change-admin-password --domain_name domain1 && \
    asadmin start-domain && \
    echo "AS_ADMIN_PASSWORD=${PASSWORD}" > /tmp/glassfishpwd && \
    asadmin --user=admin --passwordfile=/tmp/glassfishpwd enable-secure-admin && \
    asadmin --user=admin --passwordfile=/tmp/glassfishpwd delete-jvm-options -client && \
    asadmin --user=admin --passwordfile=/tmp/glassfishpwd enable-secure-admin && \
    asadmin --user=admin --passwordfile=/tmp/glassfishpwd create-jvm-options "-Dfile.encoding=UTF-8" && \
    asadmin --user=admin --passwordfile=/tmp/glassfishpwd create-jvm-options "-server" && \
    asadmin --user=admin stop-domain && \
    rm /tmp/glassfishpwd

# Ports being exposed
EXPOSE 4848 8080 8181

# Start asadmin console and the domain
CMD ["asadmin", "start-domain", "-v"]
