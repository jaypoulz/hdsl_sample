// Define the openshift pod name, docker repo url, namespace, and service acct
// for the DSL pod template.
dslPodName = "contraDsl-${UUID.randomUUID()}"
dockerRepoURL = '172.30.1.1:5000'
openshiftNamespace = 'redhat-multiarch-qe'
openshiftServiceAccount = 'jenkins'

// Create the DSL podTemplate
createDslContainers podName: dslPodName,
                    dockerRepoURL: dockerRepoURL,
                    openshiftNamespace: openshiftNamespace,
                    openshiftServiceAccount: openshiftServiceAccount,

// Pass the remainder of your jenkinsfile as a closure to the createDslContainers method
{
  node(dslPodName){
      stage("pre-flight"){
          deleteDir()
          git branch: 'master', url: 'https://github.com/jaypoulz/hdsl_sample'
      }

      stage("Parse Configuration"){
          parseConfig()
          echo env.configJSON
      }

      stage("Deploy Infra"){
          deployInfra verbose: true
      }

      stage("Configure Infra"){
          configureInfra verbose: true
      }

      // stage("Execute Tests"){
      //     executeTests verbose: true
      // }

      stage("Destroy Infra"){
          destroyInfra verbose: true
      }
      archiveArtifacts allowEmptyArchive: true, artifacts: '*, linchpin/*, resources/*, linchpin/resources/*'
  }
}
