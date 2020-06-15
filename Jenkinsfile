pipeline {
  agent any
  environment {
      CI_JOB_DATE = """${sh(
          returnStdout: true,
          script: 'date "+%Y%m%d"'
      )}"""
  }
  stages {
    // stage ('checkout scm') {
    //   steps {
    //       checkout(scm)
    //   }
    // }
    stage("测试") {
      steps {
        echo "${env.BRANCH_NAME}"
        echo "${env.PROJECT_NAME}"
        echo "自动化测试执行中..."
        echo "自动化测试执行通过"
      }
    }
    stage('构建 Docker 镜像') {
      steps {
        // -${BUILD_NUMBER}
        sh "docker build -t test:${env.BRANCH_NAME}-${env.BUILD_NUMBER} ."
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
        dingtalk ()
      }
    }
  }
}


dingtalk (
    robot: '',
    type: '',
    at:[],
    atAll: false,
    title: '',
    text:[],
    messageUrl: '',
    picUrl:'',
    singleTitle:'',
    btns: [],
    btnLayout: '',
    hideAvatar: false
)