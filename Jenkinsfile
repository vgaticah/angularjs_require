pipeline {
/* Variables de registry y credenciales para publicar la imagen de docker */ 
   environment {
	registry = "vgatica/tw"
	registryCredential = 'dockerhub'
	dockerImage = ''
   }	

  agent any    
  stages {
        
    stage('Clonando Git') {
      steps {
	git 'https://github.com/vgaticah/angularjs_require'
      }
    }
        
    stage('Install dependencies') {
      steps {
        sh 'npm install'
      }
    }
     
    stage('Test') {
      steps {
	def myTestContainer = docker.image('node:4.6')
	myTestContainer.pull()
	myTestContainer.inside {
        sh 'npm install --only=dev' 
	sh 'npm test'
	}
      }
    }
    stage ('Build image') {
	steps {
	  dockerImage =  docker.build registry + ":$BUILD_NUMBER"
     }
    }
   
  }
post {
        success {
          mail to: "vgatica@gmail.com",
	  subject: "Exitoso: ${currentBuild.fullDisplayName}", 
	  body: " Ejecución de Pipeline exitosa."
	}
        failure {
            mail to: "vgatica@gmail.com", 
	    subject: "Fallido: ${currentBuild.fullDisplayName}",
	    body: "Ejecucion de Pipeline fallida"
        }
    }
}
