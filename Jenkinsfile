
pipeline {
  agent any

  environment {
    VERSION = "1.0.${env.BUILD_NUMBER}-SNAPSHOT"
  }

  options {
    skipDefaultCheckout(true)
    timestamps()
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build & Test') {
      steps {
        sh 'mvn -B -Drevision=${VERSION} clean verify'
      }
      post {
        always {
          junit testResults: 'target/surefire-reports/*.xml', allowEmptyResults: true
        }
      }
    }

    stage('Package (fat jar)') {
      steps {
        sh 'mvn -B -Drevision=${VERSION} package'
        archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
      }
    }

    stage('Publish to Nexus') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'nexus-creds', usernameVariable: 'NEXUS_USER', passwordVariable: 'NEXUS_PASS')]) {
          sh '''
cat > settings.xml <<'EOF'
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
  <servers>
    <server>
      <id>nexus</id>
      <username>${NEXUS_USER}</username>
      <password>${NEXUS_PASS}</password>
    </server>
  </servers>
</settings>
EOF
mvn -s settings.xml -DskipTests -Drevision=${VERSION} deploy
'''
        }
      }
    }
  }

  post {
    success {
      echo "Build ${env.BUILD_NUMBER} published version ${env.VERSION}"
    }
    failure {
      echo "Build failed. Check logs."
    }
  }
}
