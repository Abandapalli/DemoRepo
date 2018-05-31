pipeline {

    agent any
 	parameters {
        string(defaultValue: "abhilash.bandapalli@oracle.com", description: 'What environment?', name: 'EmailRecipient')
        // choices are newline separated
        choice(choices: 'US-EAST-1\nUS-WEST-2', description: 'What AWS region?', name: 'region')
    }
    tools {
        maven 'Maven'
        jdk 'Java'
    }
    stages {
        stage ('Checkout'){
            steps {
            checkout scm
			
            }
   	    }
        stage('Build') {
            steps {
	       sh "echo ${params.EmailRecipient}"
	       sh "echo ${params.region}"    
               sh 'mvn clean package sonar:sonar -Dsonar.host.url=http://127.0.0.1:9000/ -DproxySet=true -DproxyHost=www-proxy.us.oracle.com -DproxyPort=80'
		  //sh 'mvn clean package -DproxySet=true -DproxyHost=www-proxy.us.oracle.com -DproxyPort=80'
		   	nexusArtifactUploader(
    			nexusVersion: 'nexus3',
    			protocol: 'http',
    			nexusUrl: 'http://localhost:8080/',
    			groupId: 'com.mkyong',
    			version: '1.0-SNAPSHOT',
    			repository: 'maven-snapshots',
    			credentialsId: '4cd5b604-879d-4f34-b79f-7052ca2b2bdc',
    			artifacts: [
        		[artifactId: 'CounterWebApp',
         		classifier: '',
         		file: 'CounterWebApp/target/CounterWebApp.war',
         		type: 'war']
    			]
 		)
           
   	    }
            
        }
		
		stage ('success'){
            steps {
                script {
                    currentBuild.result = 'SUCCESS'
                }
            }
        }
		
    }
	post {
        failure {
            script {
                currentBuild.result = 'FAILURE'
            }
        }

        always {
            step([$class: 'Mailer',
                notifyEveryUnstableBuild: true,
                recipients: "${params.EmailRecipient}",
                sendToIndividuals: true])
        }
    }
}
