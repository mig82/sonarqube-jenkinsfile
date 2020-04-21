String sectionHeaderStyle = '''
	color: white;
	background: #4b758b;
	font-family: Roboto, sans-serif !important;
	padding: 5px;
	text-align: center;
'''

String separatorStyle = '''
	border: 0;
	border-bottom: 1px dashed #ccc;
	background: #999;
'''

properties([
	[
		$class: 'BuildDiscarderProperty',
		strategy: [
			$class: 'LogRotator',
			artifactDaysToKeepStr: '',
			artifactNumToKeepStr: env.MAX_ARTIFACTS_TO_KEEP?:'8',
			daysToKeepStr: '',
			numToKeepStr: env.MAX_JOBS_TO_KEEP?:'8'
		]
	],
	parameters([

		//SCM
		[
			$class: 'ParameterSeparatorDefinition',
			name: 'SCM_HEADER',
			sectionHeader: 'SCM',
			separatorStyle: separatorStyle,
			sectionHeaderStyle: sectionHeaderStyle
		],
		string(
			name: 'SCM_URL',
			defaultValue: params.SCM_URL?:'',
			description: 'The URL of the remote SCM repository'
		),

		//Sonarqube
		[
			$class: 'ParameterSeparatorDefinition',
			name: 'SQ_HEADER',
			sectionHeader: 'Sonarqube',
			separatorStyle: separatorStyle,
			sectionHeaderStyle: sectionHeaderStyle
		],
		string(
			name: 'SQ_URL',
			defaultValue: params.SQ_URL?:'',
			description: 'The URL of the Sonarqube server'
		),
		[
			$class: 'CredentialsParameterDefinition',
			name: 'SQ_TOKEN',
			credentialType: 'org.jenkinsci.plugins.plaincredentials.impl.StringCredentialsImpl',
			defaultValue: params.SQ_TOKEN?:'',
			description: "A user's token to run analyses on a project. To generate a token, browse your Sonarqube console to <em>User > My Account > Security</em>.",
			required: true
		]
	])
])
pipeline {
	agent {label "sonarqube"}
	stages {
		stage('Sonarqube') {
			steps {
				withCredentials([string(credentialsId: 'SQ_TOKEN', variable: 'sq_token')]) {
					sh """
						sonar-scanner \
						-Dsonar.projectBaseDir=. \
						-Dsonar.projectKey=wallet \
						-Dsonar.sources=. \
						-Dsonar.host.url=${params.SQ_URL} \
						-Dsonar.login=${sq_token}
					"""
				}
			}
		}
	}
}
