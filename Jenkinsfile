pipeline {
    agent any

    stages {
        stage('Init') {
            steps {
                sh 'ansible-playbook -i hosts.ini installtions.yml'
            }
        }

        stage('Build') {
            steps {
		sh 'mvn package'
            }
        }
        stage('Test') {
            steps {
		sh 'mvn test'
            }
        }
	stage('dev deploy') {
	    steps{
                echo "deploying to DEV Env "
                deploy adapters: [tomcat7(credentialsId: 'deployer', path: '', url: 'https://18.218.255.189:7777')], contextPath: null, war: '**/.*'
    }
    }
}
}
