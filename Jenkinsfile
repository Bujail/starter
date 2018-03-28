def awesomeVersion = 'UNKNOWN'

pipeline {
    agent {
        label 'jenkins-slave'
    }
    stages {
        stage('Build') {
            steps {
                sh 'chmod 755 commands'
                sh './commands'
            }
            
        }
        stage('test') {
            steps {
                sh 'docker pull gravity2017/mmp:ravi03'
                
                script {
                   awesomeVersion = sh(returnStdout: true, script: 'docker run -d -p 80 gravity2017/mmp:ravi03')
                   
                } 
                
               echo "awesomeVersion: ${awesomeVersion}"
               
               input 'Ready to go'
                
                
                 }
             
            }
            
            
        stage('Deploy') {
            steps {
                sh 'cat ~/my_password.txt | docker login -u gravity2017 --password-stdin'
                sh 'docker pull gravity2017/mmp:ravi03'
                sh 'docker run -it -d gravity2017/mmp:ravi03'
            }
             
        }
    }
    post {
      always {
        cobertura autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: 'coverage.xml', conditionalCoverageTargets: '70, 0, 0', failUnhealthy: false, failUnstable: false, lineCoverageTargets: '80, 0, 0', maxNumberOfBuilds: 0, methodCoverageTargets: '80, 0, 0', onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false
      }
   } 
}
