@Library('github.com/piyush-garg/osio-pipeline@new-app-change') _

osio {

  config runtime: 'node'

  ci {

    def resources = processTemplate(params: [
          release_version: "1.0.${env.BUILD_NUMBER}"
    ])

    build resources: resources

  }

  cd {
    
    sh "oc version"
    
    sh "rm -rf .openshiftio"

    sh "mkdir -p .openshiftio"
    
    sh "oc new-app . -o yaml --strategy=source --dry-run=true > .openshiftio/application.yaml"

    sh "cat .openshiftio/application.yaml"

    def resources = loadResources(file: ".openshiftio/application.yaml", validate: false)

    build resources: resources

    deploy resources: resources, env: 'stage'

    deploy resources: resources, env: 'run', approval: 'manual'

  }
}
