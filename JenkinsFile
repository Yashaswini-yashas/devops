node {
   def maven
   stage('Prepare') {
      git 'git@github.com:US1E007/devops.git'
      maven = tool 'maven'
   }
   stage('Build') {
      if (isUnix()) {
         sh "'${maven}/bin/mvn' -Dmaven.test.failure.ignore clean package"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
      }
   }
   stage('Unit Test') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
   }
   stage('Integration Test') {
     if (isUnix()) {
        sh "'${maven}/bin/mvn' -Dmaven.test.failure.ignore clean verify"
     } else {
        bat(/"${maven}\bin\mvn" -Dmaven.test.failure.ignore clean verify/)
     }
   }
   stage('Sonar') {
      if (isUnix()) {
         sh "'${maven}/bin/mvn' sonar:sonar"
      } else {
         bat(/"${maven}\bin\mvn" sonar:sonar/)
      }
   }
   stage('Deploy') {
       sh 'curl -u admin:password -T target/**.war "http://52.250.55.110:7070/manager/text/deploy?path=/devops&update=true"'
   }
   stage("Smoke Test"){
       sh "curl --retry-delay 10 --retry 5 http://52.250.55.110:7070/devops"
   }
}
