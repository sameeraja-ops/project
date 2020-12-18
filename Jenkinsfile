pipeline {
    agent any

    stages {
        stage('Init') {
            steps {
                sshPublisher(publishers: [sshPublisherDesc(configName: 'tomcat_server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook -i hosts.ini installtions.yml', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
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
                deploy adapters: [tomcat7(credentialsId: 'deployer-creds', path: '', url: 'https://18.220.115.207:7777')], contextPath: null, war: 'target/*.war'
    }
    }
       stage('sonar test') {
	    steps{
		sshPublisher(publishers: [sshPublisherDesc(configName: 'tomcat_server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''mvn sonar:sonar \\
                -Dsonar.host.url=http://18.220.115.207:9000 \\
                -Dsonar.login=e659d1a83c1b6b709431a4f6c0349884cd0137b5''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])    
	    }
       }
}
}
