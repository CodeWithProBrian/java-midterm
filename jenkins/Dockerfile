
FROM jenkins/jenkins:lts-jdk17

USER root
RUN apt-get update && apt-get install -y maven && rm -rf /var/lib/apt/lists/*

# Optional: preinstall common plugins (Pipeline, Git, Maven Integration)
RUN jenkins-plugin-cli --plugins     workflow-aggregator:596.v8c21c963d92d     git:5.5.2     configuration-as-code:1932.vb_5566b_86c47b_     credentials:1309.v8835f3c7679b_     plain-credentials:179.vc5cb_98f6db_38     credentials-binding:677.vdc9d31f49589     junit:1302.vdf2f9eb_a_159f

USER jenkins
