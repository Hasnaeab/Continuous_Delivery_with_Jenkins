pipeline {
    agent any

    tools {
        maven "MAVEN3"  // Ensure MAVEN3 is configured in Jenkins tools
        jdk "OracleJDK8"  // Ensure OracleJDK8 is configured in Jenkins tools
    }

    environment {
        SNAP_REPO = 'vprofile-snapshot'
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'nexus'
        RELEASE_REPO = 'vprofile-release'
        CENTRAL_REPO = 'vpro-maven-central'
        NEXUSIP = '172.31.10.139'
        NEXUSPORT = '8081'
        NEXUS_GRP_REPO = 'vpro-maven-group'
        NEXUS_LOGIN = 'nexuslogin'
    }

    stages {
        // Build stage: Compile the project using Maven
        stage('Build') {
            steps {
                script {
                    // Ensure the settings.xml is available, else adjust the path
                    sh 'mvn -s settings.xml -DskipTests install'
                }
            }
            post {
                success {
                    echo "Build successful. Archiving build artifacts."
                    archiveArtifacts artifacts: '**/*.war', allowEmptyArchive: true
                }
            }
        }

        // Test stage: Run unit tests with Maven
        stage('Test') {
            steps {
                script {
                    // Run tests using Maven
                    sh 'mvn test'
                }
            }
        }

        // Checkstyle Analysis stage: Run code style checks
        stage('Checkstyle Analysis') {
            steps {
                script {
                    // Run Checkstyle analysis
                    sh 'mvn -s settings.xml checkstyle:checkstyle'
                }
            }
        }
    }
}
