pipeline {
  agent any
  stages {
    // stage ('checkout scm') {
    //   steps {
    //       checkout(scm)
    //   }
    // }
    stage("测试") {
      steps {
        echo "自动化测试执行中..."
        echo "自动化测试执行通过"
      }
    }
    stage('构建 Docker 镜像') {
      steps {
        sh "docker build  -t test:SNAPSHOT-${env.BRANCH_NAME}-$BUILD_NUMBER ."
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