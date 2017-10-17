properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '10')), pipelineTriggers([pollSCM('H/2 * * * *')])])

def buildNodeLabel = env.BUILD_TAG

//Kubernetes podTemplate
podTemplate( // Open Kubernetes podTemplate parameters
    name: 'build-slave',
    label: buildNodeLabel,
    containers:[
        /* Inside container templates, we use 'ttyEnabled: true' and
         * 'command: 'cat'' to prevent the container from exiting early
         * See https://github.com/jenkinsci/kubernetes-plugin#constraints
         */
        containerTemplate(
            name: 'maven',
            image: 'maven:3-alpine',
            workingDir: '/home/jenkins',
            alwaysPullImage: false,
            privileged: false,
            ttyEnabled: true,
            command: 'cat')
  ]
) // Close Kubernetes podTemplate parameters
{ // Open Kubernetes podTemplate body

    node(buildNodeLabel) {
		Integer TimeOutMinutes = 10
		stage('build') {
            timeout(TimeOutMinutes) {
                container('maven'){
                     sh 'mvn -B -DskipTests clean package' 
                }
            }
        }
	}
}
