pipeline {
agent any
stages 
{
 stage("Compile") {
 steps {
 sh "./gradlew compileJava"
 }
}
stage("Unit Test") {
steps {
sh "./gradlew test"
}
}
stage("Code coverage") {
      steps {
        sh "./gradlew jacocoTestReport"
        publishHTML (target: [
               reportDir: 'build/reports/jacoco/test/html',
               reportFiles: 'index.html',
               reportName: "JaCoCo Report" ])
        sh "./gradlew jacocoTestCoverageVerification"
      }
    }
stage("Static code analysis") {
      steps {
        sh "./gradlew checkstyleMain"
        publishHTML (target: [
               reportDir: 'build/reports/checkstyle/',
               reportFiles: 'main.html',
               reportName: "Checkstyle Report" ])
      }
    }
stage("Build") {
      steps {
        sh "./gradlew build"
      }
    }

    stage("Docker build") {
      steps {
        sh "docker build -t vishnu-janarthanan/calculator:${BUILD_TIMESTAMP} ."
      }
    }

    stage("Docker login") {
      steps {
        withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'leszko',
                          usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
          sh "docker login --username $vishnuselva --password $Selva143"
        }
      }
    }

    stage("Docker push") {
      steps {
        sh "docker push vishnu-janarthanan/calculator:${BUILD_TIMESTAMP}"
      }
    }
 }
}
