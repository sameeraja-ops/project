pipeline {
    agent any

    stages {
        stage('init') {
            steps {
                sh 'ansible-playbook -i /home/centos/host.ini /home/centos/installtions.yml'
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
