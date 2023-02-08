# Argo CD - Declarative GitOps CD for Kubernetes

Argo CD is a declarative, GitOps continuous delivery tool for Kubernetes.

## Example Project
This repo contains an example web app used to demonstrate the principles and processes of an Argo CD GitOps setup.

## Installation
The following instructions have been tested with the latest version of OpenShift Local (4.11.18).

1. Install OpenShift Local

2. Install Red Hat OpenShift GitOps Operator

2.1. Add **cluster-admin** rights to the `openshift-gitops-argocd-application-controller` service account in the `openshift-gitops` namespace:

```
oc adm policy add-cluster-role-to-user cluster-admin -z openshift-gitops-argocd-application-controller -n openshift-gitops
```

2.2. Retrieve the Argo CD web administration console URL:

```
argoURL=$(oc get route openshift-gitops-server -n openshift-gitops -o jsonpath='{.spec.host}{"\n"}')
echo $argoURL
```

2.3. Retrieve the Argo CD default password:

```
argoPass=$(oc get secret/openshift-gitops-cluster -n openshift-gitops -o jsonpath='{.data.admin\.password}' | base64 -d)
echo $argoPass
```

2.4. Login into the Argo CD web administration console as user **admin** with the password retrieved above.

3. Fork this GitHub repo

4. Git clone your forked repo locally

5. Navigate into the `argocd-gitops` directory

6. Update the `repoURL` field within the `argo-application.yaml` file with your newly forked repo URL. Save the updated `argo-application.yaml` file.

7. Create the Argo CD Application within your OpenShift cluster:

```
oc apply -f argo-application.yaml
```

8. Within the Argo CD web administration console, confirm that a new Application has been registered.

9. Confirm that Argo CD has deployed the manifests stored in the GitHub repo into the cluster successfully:

```
oc project cloudacademy
oc get pods
oc rollout status deployment webecho
```

10. Perform a direct change to the **webecho** deployment

```
oc set env deploy/webecho BACKGROUND_COLOR=cyan MESSAGE="follow the white rabbit..."
```
