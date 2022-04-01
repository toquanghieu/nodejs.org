List<String> list
podTemplate(label: 'common-pod', containers: [
        containerTemplate(name: 'docker', image: 'public.ecr.aws/smartlog/docker:19.03.8', command: 'cat', ttyEnabled: true),
        containerTemplate(name: 'kubectl', image: 'public.ecr.aws/smartlog/roffe/kubectl:v1.13.2', command: 'cat', ttyEnabled: true),
        containerTemplate(name: 'awscli', image: 'public.ecr.aws/smartlog/atlassian/pipelines-awscli:1.18.190', command: 'cat', ttyEnabled: true),
],
        volumes: [
                hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'),
        ]) {
  node('common-pod') {
    list = new ArrayList<String>(Arrays.asList(CLUSTERS.split(",")))
    deployToK8s(true, list)
  }
}
def deployToK8s(running = true, list = []) {
  if (running) {
    container('kubectl') {
      for(item in list){
        echo item
        withCredentials([file(credentialsId: item, variable: 'KUBECRED')]) {
          sh 'use $KUBECRED'
          sh 'cat $KUBECRED'
          sh 'export KUBECONFIG=$(pwd)/kubeconfig'
          sh 'grep /home/jenkins/agent/workspace/hieu-test-pipeline/kubeconfig'
          sh 'kubectl config view'
        }
      }
    }
  }
}
