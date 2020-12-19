pipeline {
    agent any

    stages {
     stage('kops setup') {
            steps {
		    sh ' echo "$(pwd)" '
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



