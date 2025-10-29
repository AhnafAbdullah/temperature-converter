pipeline {
  agent any

  tools {
    // name must match a configured NodeJS installation in Jenkins (Global Tool Configuration)
    nodejs 'NodeJS'
  }

  parameters {
    string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Branch to build from')
    string(name: 'STUDENT_NAME', defaultValue: 'Ahnaf Abdullah', description: 'Ahnaf Abdullah')
    choice(name: 'ENVIRONMENT', choices: ['dev', 'qa', 'prod'], description: 'Select environment')
    booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Run Jest tests after build')
  }

  environment {
    APP_VERSION = "1.0.${BUILD_NUMBER}"
    MAINTAINER = "Student"
  }

  stages {
    stage('Checkout') {
      steps {
        echo "Checking out branch: ${params.BRANCH_NAME}"
        // If your pipeline is running from a repository multibranch or with SCM configured,
        // checkout scm is fine. If you need to explicitly checkout a branch from a remote,
        // replace with a git checkout step.
        checkout scm
      }
    }

    stage('Install Dependencies') {
      steps {
        echo "Installing required packages..."
        bat 'npm install'
      }
    }

    stage('Build') {
      steps {
        echo "Building version ${APP_VERSION} for ${params.ENVIRONMENT} environment"
        bat """
          echo Simulating build process...
          if not exist build mkdir build
          copy src\\*.js build >nul 2>&1 || echo No .js files to copy
          echo Build completed successfully!
          echo App version: %APP_VERSION% > build\\version.txt
        """
      }
    }

    stage('Test') {
      when {
        expression { return params.RUN_TESTS }
      }
      steps {
        echo "Running Jest tests..."
        bat 'npm test'
      }
    }

    stage('Package') {
      steps {
        echo "Creating zip archive for version ${APP_VERSION}"
        // Use Powershell Compress-Archive to create the zip
        bat "powershell -Command \"Compress-Archive -Path build\\* -DestinationPath build_${env.APP_VERSION}.zip -Force\""
      }
    }

    stage('Deploy (Simulation)') {
      steps {
        echo "Simulating deployment of version ${APP_VERSION} to ${params.ENVIRONMENT}"
      }
    }
  }

  post {
    always {
      echo "Cleaning up workspace..."
      deleteDir()
    }
    success {
      echo "Pipeline succeeded! Version ${APP_VERSION} built and tested. Name: Ahnaf Abdullah"
    }
    failure {
      echo "Pipeline failed! Check console output for details."
    }
  }
}
