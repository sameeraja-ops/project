pipeline {
    agent any

    stages {
        stage('Init') {
            steps {
                sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible_server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook -i hosts.ini installtions.yml', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])            }
        }

        stage('Build') {
            steps {
		sh 'mvn package'
            }
        }

       stage('dev deploy') {
	        steps{
	           sh "echo 'deploying to DEV Env' "
                   deploy adapters: [tomcat7(credentialsId: 'deployer-creds', path: '', url: 'http://18.222.28.128:7777/')], contextPath: null, war: 'target/*.war'            
		}
    }
       stage('sonar test') {
	     steps{
               sshPublisher(publishers: [sshPublisherDesc(configName: 'tomcat_server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''cd /home/centos/project || (git clone https://github.com/sameeraja-ops/project.git && cd /home/centos/project) ;
               git pull --all ; 
               mvn compile sonar:sonar -Dsonar.host.url=http://18.222.28.128:9000 -Dsonar.login=e659d1a83c1b6b709431a4f6c0349884cd0137b5''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true)])
	    }
       }
      stage('nexus deploy') {
          steps{
            sshPublisher(publishers: [sshPublisherDesc(configName: 'tomcat_server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''cd /home/centos/project ;
            mvn deploy ;''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])          }
      } 
     stage('docker build') {
	     steps{
		     sshPublisher(publishers: [sshPublisherDesc(configName: 'tomcat_server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''(docker stop sam-finalcontainer && docker rm -rf sam-finalcontainer) || echo "Container specified doesnt exist" ;
                     docker build -t  sam-finalimage /home/centos/project/ ; docker run -dt --name sam-finalcontainer -p 9094:8080 sam-finalimage ; docker tag sam-finalimage sameeraja/sam-finalimage ; ''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
	     }
     }
     stage('docker push') {
	     steps {
		     sshPublisher(publishers: [sshPublisherDesc(configName: 'tomcat_server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''docker login --username=sameeraja --password=Sameera123$
                     docker push sameeraja/sam-finalimage''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
	     }
     }
     stage('kops setup') {
            steps {
                    sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible_server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook -i hosts.ini kops-setup.yml', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true)])            
	    }
        }	    
     stage('kubernetes deployment') {
	     steps {
		     sshPublisher(publishers: [sshPublisherDesc(configName: 'kubernetes_server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'kubectl apply -f /home/centos/project/deployment.yml', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true)])
	     }
     }
}
}



