pipeline {
    agent any
    tools {
        maven "Maven"
    }

    stages {
        stage("gitcheckout") {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'gitcredentials', url: 'https://github.com/rajgit2021/MavenSonarNexusProject.git']]])
            }
        }
        stage("Build")
        {
            steps {
                echo "Sonar started"
                bat "mvn clean install"
            }
        }
        stage("Sonar_Build")
        {
            steps {
                echo "Sonar started"
                bat "mvn sonar:sonar"
            }
        }
        stage("Nexus_Build")
        {
            steps {
                echo "Nexus started"
                //bat "mvn clean install -P release"
				bat "mvn clean deploy -P release"
            }
        }
		stage("Deploy To Container") {
            steps {
              deploy adapters: [tomcat8(credentialsId: 'tomcat', path: '', url: 'http://localhost:8080/')], contextPath: 'MavenSonarNexusProject', war: '**/*.war'
            }
        }
    }
}