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
		)
	])
])
pipeline {
	agent any
	stages {
		stage('Sonarqube') {
			steps {
				sh '''
					sonar-scanner \
					-Dsonar.projectBaseDir=. \
					-Dsonar.projectKey=wallet \
					-Dsonar.sources=. \
					-Dsonar.host.url=${params.SQ_URL} \
					-Dsonar.login=foo
				'''
			}
		}
	}
}
