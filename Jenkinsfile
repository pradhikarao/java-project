pipeline {

  agent none

  stages {
    stage('Unit Tests'){
      agent {
        label 'apache'
      }
      steps {
        sh 'ant -f test.xml -v'
        junit 'reports/result.xml'
      }

    }
    stage('build'){
      agent {
        label 'apache'
      }
      steps {
      sh 'ant -f build.xml -v'
    }
    post {
      always {
        archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
    }
  }
    }

    stage('deploy to apache'){
      agent {
        label 'apache'
      }
      steps {
        sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all"
      }
    }

    stage('Running on CentOS'){
      agent {
        label 'CentOS'
      }
      steps {
        sh "wget http://pradhikarao1.mylabserver.com/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar"
        sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 2 5"
      }
    }

    stage('Test on Debian') {
      agent {
        docker 'openjdk:8u121-jre'
      }
      steps {
        sh "wget http://pradhikarao1.mylabserver.com/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar"
        sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 2 5"
      }
    }
  }


}
