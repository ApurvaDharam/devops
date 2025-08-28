node {
    stage('Build') {
        withEnv([
            "JAVA_HOME=${tool 'JDK21'}",
            "PATH+JAVA=${tool 'JDK21'}/bin",
            "PATH+MAVEN=${tool 'Maven 3'}/bin"
        ]) {
            sh 'mvn clean package'
        }
    }

    stage('Test') {
        withEnv([
            "JAVA_HOME=${tool 'JDK21'}",
            "PATH+JAVA=${tool 'JDK21'}/bin",
            "PATH+MAVEN=${tool 'Maven 3'}/bin"
        ]) {
            sh 'mvn test'
        }
        // even if no test results, don't fail
        junit allowEmptyResults: true, testResults: '**/target/surefire-reports/*.xml'
    }

    stage('Archive') {
        archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
    }
}
