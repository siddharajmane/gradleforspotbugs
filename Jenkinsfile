pipeline {
    agent any

    triggers {
        pollSCM('*/5 * * * *')
    }

    stages {
        stage('Compile') {
            steps {
                gradlew('clean', 'classes')
            }
        }
        stage('Unit Tests') {
            steps {
                gradlew('test')
            }
        }
    }
    post {
        success{
            def spotbugs = scanForIssues tool: spotBugs(pattern: '**/Spotbug-Reports/**/main.xml')
        	publishIssues issues: [spotbugs] 
        }

    }
}

def gradlew(String... args) {
    sh "./gradlew ${args.join(' ')} -s"
}
