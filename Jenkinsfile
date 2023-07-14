node{
    
    def tag, dockerHubUser, containerName, httpPort = ""
    
    stage('Prepare Environment'){
        echo 'Initialize Environment'
        tag="3.0"
	withCredentials([usernamePassword(credentialsId: '787a04c1-0ab0-4e68-9224-f513f8fb73a4', usernameVariable: 'susha15', passwordVariable: 'dckr_pat_rc4xvHuOIhFKd_k4h1f_RRm2TRg')]) {
		dockerHubUser="susha15"
        }
	containerName="bankingapp"
	httpPort="8989"
    }
    
     stage('Checkout Source') {
      steps {
        git 'https://github.com/Susha15/BankingSpringBootApplication.git'
      }
    }
    
    stage('Maven Build'){
        sh "mvn clean package"        
    }
    
    stage('Docker Image Build'){
        echo 'Creating Docker image'
        sh "docker build -t susha15/bankingapp --pull --no-cache ."
    }  
	
    stage('Publishing Image to DockerHub'){
        echo 'Pushing the docker image to DockerHub'
        withCredentials([usernamePassword(credentialsId: '787a04c1-0ab0-4e68-9224-f513f8fb73a4', usernameVariable: 'susha15', passwordVariable: 'dckr_pat_rc4xvHuOIhFKd_k4h1f_RRm2TRg')]) {
		sh "docker login -u susha15 -p dckr_pat_rc4xvHuOIhFKd_k4h1f_RRm2TRg"
		sh "docker push susha15 /bankingapp"
		echo "Image push complete"
        } 
    }    
	
	stage('Ansible Playbook Execution'){
		sh "ansible-playbook -i inventory.yaml kubernetesDeploy.yaml -e httpPort=8989 -e containerName=bankingapp -e dockerImageTag=susha15/bankingapp:swagger"
	}
}


