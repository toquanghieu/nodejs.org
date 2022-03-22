podTemplate(label: 'common-pod', containers: [
    containerTemplate(name: 'docker', image: 'public.ecr.aws/smartlog/docker:19.03.8', command: 'cat', ttyEnabled: true),
    containerTemplate(name: 'kubectl', image: 'public.ecr.aws/smartlog/roffe/kubectl:v1.13.2', command: 'cat', ttyEnabled: true),
    containerTemplate(name: 'awscli', image: 'public.ecr.aws/smartlog/atlassian/pipelines-awscli:1.18.190', command: 'cat', ttyEnabled: true),
  ],
  volumes: [
    hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'),
  ]) {
    node('common-pod') {
      List<String> list = new ArrayList<String>(Arrays.asList(CLUSTERS.split(",")));
      deployToK8s();
    }
  }
def deployToK8s(running = true) {
  if (running) {
    container('kubectl') {
      for(item in list){
        echo item
        def MY_KUBECONFIG = credentials(item)
        sh """
                kubectl --kubeconfig $MY_KUBECONFIG 
                kubectl kubectl config view
            """
      }
    }
  }
}
