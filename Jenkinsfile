pipeline {
  agent any
  stages {
    stage("检出") {
      steps {
        checkout([
          $class: 'GitSCM',
          branches: [[name: env.GIT_BUILD_REF]],
          userRemoteConfigs: [[
            url: env.GIT_REPO_URL,
            credentialsId: env.CREDENTIALS_ID
        ]]])
      }
    }

    stage("测试和构建") {
      steps {
        echo "自动化测试执行中..."
        echo "自动化测试执行通过"

        echo "构建中..."
        echo "构建成功"
      }
    }

    stage('人工确认') {
      input {
        message "是否部署到测试环境中"
        submitter "{admin,hichenxinyu}"
        parameters {
          choice(
            name: "testing_env",
            choices: ["testing-1", "testing-2", "testing-3"],
            description: "请选择部署的测试环境目标"
          )
        }
      }
      steps {
        echo "您希望部署的测试环境为 ${testing_env}"
      }
    }

    stage('部署') {
      steps {
        echo "部署中..."
        echo "部署成功"
      }
    }
  }
}