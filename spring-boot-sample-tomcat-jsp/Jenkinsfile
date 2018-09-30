node {
    try{
        notify('Started')
        stage('Downloading') {
        git 'https://github.com/Dhamodharanraju/springBoot.git'
        }
        
        dir('spring-boot-samples/spring-boot-sample-tomcat-jsp/') {
            stage('Clean') {
            sh 'mvn clean'
            }
            stage('Compile') {
            sh 'mvn clean'
            }
            stage('Test') {
            sh 'mvn compile'
            }
            stage('Package') {
            sh 'mvn package'
            }
			stage('Build Docker Image'){
			sh 'sudo docker build -t limbrit/my-app001:0.0.1 .'
			}
			stage('Push Docker Image'){
			withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerhub')]) {
				sh "sudo docker login -u limbrit -p ${dockerhub}"
				}
   				sh 'sudo docker push limbrit/my-app001:0.0.1'
			}
			stage('Running Docker on Local'){
				sh 'sudo docker run -p 8085:8085 -d limbrit/my-app001:0.0.1'
			}
            archiveArtifacts 'target/*.war'
        }
        notify('Success')
    } catch (err) {
        notify("Error ${err}")
        currentBuild.result = 'FAILURE'
    }   
}
def notify(status){
    emailext (
      to: "limbritjava@gmail.com",
      subject: "${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
      body: """<p>${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
        <p>Check console output at <a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a></p>""",
    )
}