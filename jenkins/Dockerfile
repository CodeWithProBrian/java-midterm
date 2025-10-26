FROM jenkins/jenkins:lts-jdk17

# Need root to install packages
USER root
RUN apt-get update && apt-get install -y maven && rm -rf /var/lib/apt/lists/*

# Preinstall core plugins (no version pins to avoid update-center mismatches)
RUN jenkins-plugin-cli --plugins \
    workflow-aggregator \
    git \
    configuration-as-code \
    credentials \
    plain-credentials \
    credentials-binding \
    junit

# Back to the jenkins user
USER jenkins
