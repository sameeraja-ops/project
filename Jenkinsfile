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
	
}
}
