## What is the image for?
The intended purpose of this image is for it to be used as a Jenkins agent. By using the installed features the user is able to create Jenkins pipelines that can deploy applications and services using OpenShift. An example of using this image as a Jenkins agent via [Kubernetes](https://plugins.jenkins.io/kubernetes/) can be seen below. 

First, an example of configuring the pod template in yaml to create the agent.

```yaml
jenkins:
  clouds:
    - kubernetes:
        name: "kubernetes"
        templates:
          - name: "image-builder-openshift"
            label: "image-builder-openshift"
            nodeUsageMode: NORMAL
            containers:
              - name: "image-openshift"
                image: "ghcr.io/liatrio/image-builder-openshift:${builder_images_version}"
```
And then specifying the agent in the Jenkinsfile for an example step.

```jenkins
stage('Build') {
  agent {
    label "image-builder-openshift"
  }
  steps {
    container('image-openshift') {
      sh "oc cluster up"
    }
  }
}
```

## What is installed on this image?
- Version [3.11.0](https://github.com/openshift/origin/tree/release-3.11) of OKD.
- Version [3.9.4](https://hub.docker.com/layers/alpine/helm/3.9.4/images/sha256-b1883b29f2d04ab86cc4ac052ac8a897fd360e16bbec9dcb37268bcca7d26ed8?context=explore) of Helm.
