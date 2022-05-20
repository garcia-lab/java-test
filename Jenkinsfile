pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        echo 'Stage: Checkout (Start)'
        checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://ghe.io/colossus9-test-org/java-test.git']]])
        echo 'Stage: Checkout (End)'
      }
    }
    stage('Build') {
      steps {
        echo 'Stage: Build (Start)'
        bat 'mvn -T 3 clean install'
        echo 'Stage: Build (End)'
      }
    }
    stage('Get input') {
      input {
        message 'Should we continue?'
        id 'Yes, we should.'
        submitter 'Anitha s,bob'
        parameters {
          string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
        }
      }
      steps {
        echo 'Stage: Get input (Start)'
        echo "Hello, ${PERSON}, nice to meet you."
        echo 'Stage: Get input (Done)'
      }
    }
    stage('Publish Junit') {
      steps {
        echo 'Stage: Publish Junit (Start)'
        cobertura(coberturaReportFile: 'target/site/cobertura/coverage.xml', conditionalCoverageTargets: '70, 0, 0', lineCoverageTargets: '80, 0, 0', methodCoverageTargets: '80, 0, 0', sourceEncoding: 'ASCII')
        echo 'Stage: Publish Junit (End)'
      }
    }
    stage('Parallel') {
      parallel {
        stage('Coverage') {
          steps {
            echo 'Stage: Coverage (Start)'
            echo 'Stage: Coverage (End)'
          }
        }
        stage('Parallel stage') {
          steps {
            echo 'Stage: Parallel stage (Start)'
            echo 'Stage: Parallel stage (End)'
          }
        }
      }
    }
  }
  tools {
    maven 'maven-auto'
  }
  post {
    always {
      echo 'Reached the post section'
    }
  }
  options {
    timeout(time: 1, unit: 'HOURS')
  }
  parameters {
    string(name: 'PERSON', description: 'Who should I say hello to?')
    booleanParam(name: 'DEBUG_BUILD', description: '')
  }
  triggers {
    cron('H */4 * * 1-5')
  }
}
