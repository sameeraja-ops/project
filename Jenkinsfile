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

       stage('dev deploy') {
	        steps{
	           sh "echo 'deploying to DEV Env' "
                   deploy adapters: [tomcat7(credentialsId: 'deployer-creds', path: '', url: 'http://3.22.168.223:7777/')], contextPath: null, war: 'target/*.war'            
		}
    }
       stage('sonar test') {
	     steps{
               sshPublisher(publishers: [sshPublisherDesc(configName: 'tomcat_server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''cd /home/centos/project || (git clone https://github.com/sameeraja-ops/project.git && cd /home/centos/project) ;
               git pull --all ; 
               mvn compile sonar:sonar -Dsonar.host.url=http://3.22.168.223:9000 -Dsonar.login=e659d1a83c1b6b709431a4f6c0349884cd0137b5''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true)])
	    }
       }
      stage('nexus deploy') {
          steps{
            sshPublisher(publishers: [sshPublisherDesc(configName: 'tomcat_server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''cd /home/centos/project ;
            mvn deploy ;''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])          }
      } 
     stage('docker build') {
	     steps{
		     sshPublisher(publishers: [sshPublisherDesc(configName: 'tomcat_server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''(docker stop sam-containers1 && docker rm -rf sam-containers1) || echo "Container specified doesnt exist" ;
                     docker build -t  sam-images1 /home/centos/project/ ; docker run -dt --name sam-containers1 -p 9093:8080 sam-images1 ; docker tag sam-images1 sameeraja/sam-images1 ; ''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
	     }
     }
}
}



