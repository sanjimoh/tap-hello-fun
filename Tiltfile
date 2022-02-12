allow_k8s_contexts('arn:aws:eks:ap-south-1:039975237480:cluster/msanjit-feb-demo')
SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='index.docker.io/sanjitun/tap-hello-fun-source')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='msanjit')

k8s_custom_deploy(
    'tap-hello-fun',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --live-update" +
              " --local-path " + LOCAL_PATH +
              " --source-image " + SOURCE_IMAGE +
              " --namespace " + NAMESPACE +
              " --yes >/dev/null" +
              " && kubectl get workload tap-hello-fun --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=['pom.xml', './target/classes'],
    container_selector='workload',
    live_update=[
      sync('./target/classes', '/workspace/BOOT-INF/classes')
    ]
)

k8s_resource('tap-hello-fun', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'serving.knative.dev/service': 'tap-hello-fun'}])