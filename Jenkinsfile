pipeline {
  agent any

  tools {
    nodejs "node24" // name must match the one you configured
  }

  parameters {
    string(name: 'DEPLOY_VERSION', defaultValue: 'v1.0.0', description: 'Release version')
    string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Git branch to deploy')
    choice(name: 'ENV', choices: ['dev', 'staging', 'prod'], description: 'Target environment')
  }

  environment {
    APP_NAME = "react-demo"
    BUILD_DIR = "build"
  }

  stages {
    stage('Checkout') {
      steps {
        echo "📦 Checking out branch: ${params.BRANCH_NAME}"
        git branch: "${params.BRANCH_NAME}",
            url: 'https://github.com/Sampada-09/my-app.git',
            credentialsId: 'github-creds'
      }
    }

    stage('Install Dependencies') {
      steps {
        echo "📥 Installing dependencies"
        sh 'npm install'
      }
    }

    stage('Build') {
      steps {
        echo "🔧 Building version ${params.DEPLOY_VERSION}"
        sh 'npm run build'
      }
    }

    stage('Deploy') {
      steps {
        echo "🚀 Deploying to environment: ${params.ENV}"
        script {
          try {
            sh 'echo Simulating deploy...'
            // sh 'exit 1' // simulate failure
          } catch (err) {
            echo "❌ Deployment failed. Triggering rollback..."
            currentBuild.result = 'FAILURE'
            throw err
          }
        }
      }
    }
  }

  post {
    failure {
      echo "🔁 Rolling back deployment to last working version..."
    }
    success {
      echo "✅ Deployment succeeded: ${params.DEPLOY_VERSION} → ${params.ENV}"
    }
  }
}
