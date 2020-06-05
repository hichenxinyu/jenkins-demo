node('') {
    stage('Prepare') {
        echo "1.Prepare Stage"
        checkout scm
        sh "printenv"
        // sh "echo ${BRANCH_NAME} "
        script {
            // CI_JOB_DATE = $(date "+%Y%m%d")
            build_tag = "${BUILD_NUMBER}"
        }
    }
    stage('Test') {
      echo "2.Test Stage"
    }
    stage('Build') {
        echo "3.Build Docker Image Stage"
        sh "docker build -t cnych/jenkins-demo:${build_tag} ."
    }
    stage('人工确认') {
        input {
          message '是否部署到测试环境中'
          parameters {
            choice(name: 'testing_env', choices: ['testing-1', 'testing-2', 'testing-3'], description: '请选择部署的测试环境目标')
          }
        }
        // steps {
        //   echo "您希望部署的测试环境为"
        // }
    }
    stage('Push') {
        echo "4.Push Docker Image Stage"
        withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
            sh "docker login -u ${dockerHubUser} -p ${dockerHubPassword}"
            sh "docker push cnych/jenkins-demo:${build_tag}"
        }
    }
    stage('Deploy') {
        echo "5. Deploy Stage"
        if (env.BRANCH_NAME == 'master') {
            input "确认要部署线上环境吗？"
        }
        sh "sed -i 's/<BUILD_TAG>/${build_tag}/' k8s.yaml"
        sh "sed -i 's/<BRANCH_NAME>/${env.BRANCH_NAME}/' k8s.yaml"
        sh "kubectl apply -f k8s.yaml --record"
    }
}
