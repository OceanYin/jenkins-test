GITHUB_DOMAIN_NAME = "github.com"
GITHUB_ENDPOINT_API = "${GITHUB_DOMAIN_NAME}/api/v3"
GITHUB_PROJECT_REPO = "https://${GITHUB_ENDPOINT_API}/repos/OYin-Quest/jenkins-test"

GITHUB_SCRIPTS_AND_CONFIGURATION_CONTENTS_URL = "https://${GITHUB_ENDPOINT_API}/repos/OYin-Quest/jenkins-test/contents"
GITHUB_MAPPING_URL = "${GITHUB_SCRIPTS_AND_CONFIGURATION_CONTENTS_URL}/pipeline/src/mapping.groovy"
GITHUB_TRIGGERS_URL = "https://raw.githubusercontent.com/OYin-Quest/jenkins-test/${env.BRANCH_NAME}/groovy/triggers.groovy"


def mapping = null
def triggers = null

timestamps{
	node{
		try{
			echo GITHUB_TRIGGERS_URL
		
			withCredentials([usernameColonPassword(credentialsId: 'GitHub-OYin-Quest', variable: 'GITHUB_ACCESS_TOKEN')]) {
				response = GITHUB_TRIGGERS_URL.toURL().getText(requestProperties: ['Accept': "application/vnd.github.v3.raw"])
				echo response
				triggers = evaluate(response)
			}
		
			// groovy file will not available unless source code is cloned.
			// triggers = load "groovy/triggers.groovy"
		
			echo "hello, world"
			stage('Preparation'){
				echo 'creating vm'
				triggers.callTriggerOnBranch("on_commit", env.BRANCH_NAME)
			}
			stage('run'){
				echo 'run'
			}
			
			stage('deploy'){
				echo 'deploy'
			}
		} catch(error){
			echo "error catched"
			echo error.toString()
			throw error
		} finally{
			echo "finally"
			stage('copy'){
				echo 'copy artifacts'
			}
			stage('cleanup'){
				triggers.callTriggerOnBranch("on_approval", env.BRANCH_NAME)
				echo 'cleanup'
			}
		}
	}
}