pipeline {
    agent any 
    tools{
        maven 'Maven'
        jdk 'JDK'
    }
    stages {
        stage('clone') {
            steps {
                echo "Cloning ..."
                git 'https://github.com/vathsalahn/jenkins-demo.git'
            
            }
        }
        stage('build'){
            steps {
                echo "Building.."
                sh 'mvn -f MavenProject/pom.xml compile package'
            }
        }
        stage('archive'){
            steps {
                echo "Archiving..."
                archiveArtifacts '**/*.war'
            }
        }
        stage("reports.."){
            steps{
                parallel (
                    "FindBugs.." : {
                        node('window') {
                            sh "mkdir javancss1 ; cd javancss1 ;pwd"
                            git 'https://github.com/edureka-devops/jenkins-demo.git'
                            sh "cd javancss-master ; mvn findbugs:findbugs ; pwd"
                            deleteDir()
                        }
                    },
                    "Cobertura..." : {
                        node('window') {
                            git 'https://github.com/edureka-devops/jenkins-demo.git'
                            sh 'mvn -f MavenProject/pom.xml clean cobertura:cobertura -Pmetrics'
                        }
                    }
                ) 
            }
        }        
        stage('deploy') {
            steps {
                script {
                    echo "Deploying..."
                    sh "cp MavenProject/multi3/target/*.war 'C://Program Files//Apache Software Foundation//Tomcat 7.0//webapps//'"
                }
            }
        }
		stage('email')
        {
            steps {
                echo 'Emailing..'
                echo EMAIL_LIST
                emailext body: 'test', to: EMAIL_LIST, subject: 'Test'
            }
        }
        stage('html report') {
            steps{
                echo "Publish html report..."
                publishHTML(
                    [allowMissing: false, 
                    alwaysLinkToLastBuild: false,
                    keepAll: false,
                    reportDir: '', 
                    reportFiles: 'index.html',
                    reportName: 'HTML Report',
                    reportTitles: ''])
            }
        }
		stage('clean up') {
            steps {
                echo "Cleaning up..."
                cleanWs()
            }
        }        
    }
}
