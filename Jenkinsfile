node() {
	def sonarHome = tool name: 'SonarScanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
	
		stage('Code Checkout'){
		checkout changelog: false, poll: false, scm: scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'GitHubCreds', url: 'https://github.com/anujdevopslearn/MavenBuild']])
		}
		stage('Package'){
		sh """
			ls -lart
			mvn clean package
			ls -lart target

		"""
		}
	
		stage('Code Scan'){
		withSonarQubeEnv(credentialsId: 'SonarQubeCreds') {
			sh "${sonarHome}/bin/sonar-scanner"
		}
		}
	
		stage('Code Deployment'){
			deploy adapters: [tomcat9(credentialsId: 'TomcatCreds', path: '', url: 'http://3.109.143.48:8080/')], contextPath: 'counterwebapp', onFailure: false, war: 'target/*.war'
		}
	}
}
