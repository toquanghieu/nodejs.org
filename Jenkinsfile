def CLUSTERS = "customer1-cluster"
List<String> list
podTemplate(label: 'common-pod', containers: [
        containerTemplate(name: 'docker', image: 'public.ecr.aws/smartlog/docker:19.03.8', command: 'cat', ttyEnabled: true),
        containerTemplate(name: 'kubectl', image: 'public.ecr.aws/smartlog/roffe/kubectl:v1.13.2', command: 'cat', ttyEnabled: true),
        containerTemplate(name: 'awscli', image: 'public.ecr.aws/smartlog/atlassian/pipelines-awscli:1.18.190', command: 'cat', ttyEnabled: true),
          containerTemplate(name: 'awskubectl', image: 'bearengineer/awscli-kubectl', command: 'cat', ttyEnabled: true)
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
    container('awskubectl') {
      for(item in list){
        echo item
        withCredentials([file(credentialsId: item, variable: 'KUBECONFIG'),
                         string(credentialsId: 'AWS_ACCESS_KEY_ID', variable: 'AWS_ACCESS_KEY_ID'),
                         string(credentialsId: 'AWS_SECRET_ACCESS_KEY', variable: 'AWS_SECRET_ACCESS_KEY')]) {
          echo "${KUBECONFIG}"
          echo "${AWS_ACCESS_KEY_ID}"
          echo "${AWS_SECRET_ACCESS_KEY}"
          sh 'kubectl config view'
          sh 'kubectl get pods'
        }
      }
    }
  }
}
