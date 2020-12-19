pipeline {
    agent any

    stages {
     stage('kops setup') {
            steps {
		    sh ' echo "$(pwd)" '
		    sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible_server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''cd /home/centos/project ;
                    ansible-playbook -i /home/centos/project/hosts.ini /home/centos/project/kops-setup.yml -v
                    ''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
	    }
        }    
     stage('kubernetes deployment') {
	     steps {
                   sshPublisher(publishers: [sshPublisherDesc(configName: 'kubernetes_server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''cd /home/centos/project || (git clone https://github.com/sameeraja-ops/project.git && cd /home/centos/project) ;
                   git pull --all ;
                   kubectl apply -f /home/centos/project/deployment.yml ''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)]) 
	     }
     }
    stage('kubernetes service') {
	     steps {
		   sshPublisher(publishers: [sshPublisherDesc(configName: 'kubernetes_server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: ''' cd /home/centos/project || (git clone https://github.com/sameeraja-ops/project.git && cd /home/centos/project) ;
                   git pull --all ;
                   kubectl apply -f home/centos/project/service.yml''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true)])  
	     }	
    }
	    
}
}



