pipeline {
    agent any
    tools { 
        maven 'mymaven' 
        jdk 'myjdk' 
    }
     
    stages {
      stage('checkout') {
           steps {
             
                git branch: 'master', url: 'https://github.com/MusthafaMashkura/SmartHotel360-CouponManagement.git'
             
          }
        }
      stage('Code Analysis') {
           steps {
                withSonarQubeEnv('mysonarqube')
               {
                   sh 'mvn clean package sonar:sonar -Dsonar.java.binaries=./target/classes'
               }
           }
         }
       stage("Quality Gate result") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    waitForQualityGate abortPipeline: true
                }
            }
        }
             stage('Owasp Dependency check') {
           steps {
               dependencyCheck additionalArguments: '--format HTML', odcInstallation: 'my-dpcheck'
            }
        }
      stage('Build') {
           steps {
               sh 'mvn clean test package'
            }
        }
      stage('Check Ansible version') {
           steps {
               sh 'ansible --version'
               sh 'python --version'
            }
        }
      stage('Ansible Deploy') {
           steps {
               sh 'ls -ltrh'
               sh 'ansible-playbook -i localhost myfirstplaybook.yml'
            }
        }
    }
/*    post { 
        always { 
            cleanWs()
        }
    }*/
}
