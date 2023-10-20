# Kubernetes Manifests for Jenkins Deployment

Refer https://devopscube.com/setup-jenkins-on-kubernetes-cluster/ for step by step process to use these manifests.

However, we modified it to use longhorn as PVC and using a kube-agent as executor.
Our Pi stories will describe this in more details (to be done).

A test via Jenkins:

```bash
Started by user gratien dhaese
Running as SYSTEM
Agent kube-agent-stdzv is provisioned from template kube-agent
---
apiVersion: "v1"
kind: "Pod"
metadata:
  labels:
    jenkins: "agent"
    jenkins/label-digest: "ffa3ba115a1a18165cef0867902fabef92179d38"
    jenkins/label: "kubeagent"
  name: "kube-agent-stdzv"
  namespace: "devops-tools"
spec:
  containers:
  - env:
    - name: "JENKINS_SECRET"
      value: "********"
    - name: "JENKINS_AGENT_NAME"
      value: "kube-agent-stdzv"
    - name: "JENKINS_NAME"
      value: "kube-agent-stdzv"
    - name: "JENKINS_AGENT_WORKDIR"
      value: "/home/jenkins/agent"
    - name: "JENKINS_URL"
      value: "http://jenkins-service.devops-tools.svc.cluster.local:8080/"
    image: "jenkins/inbound-agent"
    imagePullPolicy: "IfNotPresent"
    name: "jnlp"
    resources: {}
    tty: false
    volumeMounts:
    - mountPath: "/home/jenkins/agent"
      name: "workspace-volume"
      readOnly: false
    workingDir: "/home/jenkins/agent"
  hostNetwork: false
  nodeSelector:
    kubernetes.io/os: "linux"
  restartPolicy: "Never"
  volumes:
  - emptyDir:
      medium: ""
    name: "workspace-volume"

Building remotely on kube-agent-stdzv (kubeagent) in workspace /home/jenkins/agent/workspace/test-kubeagent
[test-kubeagent] $ /bin/sh -xe /tmp/jenkins16819870548331319510.sh
+ echo testing
testing
Finished: SUCCESS
```

### References

[1] [Set-up k3s registry for jenkins](https://bryanbende.com/development/2021/07/02/k3s-raspberry-pi-jenkins-registry-p1)

[2] [Set-up jenkins on kubernetes cluster](https://devopscube.com/setup-jenkins-on-kubernetes-cluster)

[3] [Jenkins on kubernetes](https://medium.com/slalom-build/jenkins-on-kubernetes-4d8c3d9f2ece)

[4] [Set-up Jenkins build agent on kubernetes](https://devopscube.com/jenkins-build-agents-kubernetes/)
