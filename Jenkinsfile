pipeline {    
agent any     
parameters {
        string(defaultValue: "", description: 'What environment?', name: 'Environment')
        // choices are newline separated
        choice(choices: 'US-EAST-1\nUS-WEST-2', description: 'What AWS region?', name: 'region')
    }
    tools {
        maven 'Your Maven Name'
        jdk 'Your JDK Name'
    }
    stages
 { 
       stage ('Checkout')
{
            steps
             {
                git (
                    url: 'https://github.com/Abandapalli/DemoRepo.git',
                    credentialsId: 'b7574f7c-3389-410a-b3c9-21c32c9ff3f7'
                )
            }
                        }
    }
}