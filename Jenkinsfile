node ('jnode') {
//       agent {
//    //agent name 
//    jnode
//    //label 'jnode'
//    //node 'jnode'
//   }
    
    def app

    stage('Clone repository') {
      

        checkout scm
    }

    stage('Build Docker image') {
  
       app = docker.build("devopsodia-harbor.com/argocd-image-helm/devopsodiahelm:${env.BUILD_NUMBER}")
    }

    stage('Test Docker image') {
  

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image to harbor') {
        sh 'docker login -u admin -p Harbor12345 devopsodia-harbor.com'
            app.push("${env.BUILD_NUMBER}")
    }
    stage('Publish  Helm') {
    
        echo "Packing helm chart"
            sh "helm package -d ${WORKSPACE}/helm ${WORKSPACE}/helm/devopsodia"
            sh "helm registry login devopsodia-harbor.com/chartrepo/argocd-image-helm --username admin --password Harbor12345"
            sh "helm registry push ${WORKSPACE}/helm/devopsodia-1.tgz -v devopsodia-harbor.com/chartrepo/argocd-image-helm:-v"
           // sh "${WORKSPACE}/build.sh --pack_helm --push_helm --helm_repo ${HELM_REPO} --helm_usr ${HELM_USR} --helm_psw ${HELM_PSW}"
           sh "curl -u admin:Harbor12345 devopsodia-harbor.com/chartrepo/argocd-image-helm --upload-file ${WORKSPACE}/helm/devopsodia-1.tgz -v"
            
        }
    
}