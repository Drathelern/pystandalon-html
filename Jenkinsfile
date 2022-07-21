

/* 
  
  -Jenkinsfile is a file where you can write the steps or instructions 
    necessary to execute a compilation, 
    this helps us automate pipelines and execute requests on them.
 - Two Syntaxis:  Declarative and Scripted.
 - Difference between the declarative and scripted Jenkinsfiles: 
      +Declarative imposes limitations on the user with a more strict and predefined structure, 
      +scripted has few limitations regarding structure and syntax making it ideal for users with more complex requirements
  -Maven: Is a build automation tool for Java projects, It is used for projects build, dependency and documentation. 
    It simplifies the build process like ANT.
      +Ant is a Java library and command line tool whose can be used to compile your code, fetching dependencies and for packaging.
  -Lint: Lint that flag suspicious usage in software written in any computer language.
*/
pipeline {
  agent any
  stages {
     
    stage('Validate') {
      steps {
        sh 'mvn validate' /* Validates that the project is correct and all necessary information is available. This also makes sure the dependencies are downloaded. */
      }
    }

    stage('Clean') {
      steps {
        sh 'mvn clean' /* Clears the target directory into which Maven normally builds your project. */
      }
    }
    
    stage('Unit Test') {
      steps {
        sh 'mvn test' /* Runs the tests against the compiled source code using a suitable unit testing framework. 
                        These tests should not require the code be packaged or deployed. */
      }
    }
    
    stage('Lint Code'){
      steps{
        sh 'mvn checkstyle:check' /* The Checkstyle Plugin generates a report regarding the code style used by the developers. */
       }
     } 
    
     stage('Static Code analysis') { /* Conect Sonarqube scanner with SonarCloud to do continuous inspection of code quality 
                                     To the conection with SonarCloud I have my sonar-project.propierties file */
            environment {
                scannerHome = tool 'SonarCloud' 
            }
            steps {
                withSonarQubeEnv('SonarCloud') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }          
            }
    }
    
    
    stage('Quality Gate') { /* Quality Gates is a centralized solution for monitoring data quality. 
                                It helps organizations efficiently improve the quality of information.
                                SonarCloud send a post of quality gate status */
            steps {            
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
    }

  }
  tools { /* Tools used in Global tool configuration for use in the project */
    maven 'Maven-3.8.1'
    jdk 'JDK11'
  }
}
