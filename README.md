[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)

# CKAD-commands
This is a list of useful Kubernetes commands used during my prepartion for the CKAD certification, you can find the full post about the CKAD in my blog https://marcusvieira.tech.

## Exam topics and weights

- Core Concepts - 13%
- Configuration - 18%
- Multi-container pods - 10%
- Observability - 18%
- Pod design - 20%
- Services & networking - 13%
- State persistence - 8%

## Linux alias:

export ns=<namespace-name>
alias k='kubectl -n $ns'

## Set context:

kubectl config set-context <context-name>
kubectl config current-context

## Help command:

kubectl create pod -h
kubectl create deployment -h

## Generators:

kubectl run <pod-name> --image=nginx --restart=Never --labels=env=prod (Pod)
kubectl run <deployment-name> --image=nginx --replicas=3 (Deployment)
kubectl run <replica-set-name> --generator=run/v1 --image=alpine (ReplicaSet)
kubectl run <job-name> --image=job --restart=OnFailure (Job)
kubectl run <cron-job-name> --image=cron --restart=OnFailure --schedule="*/1 * * * *" (CronJob)

## Export configuration to file:

kubect get pod <pod-name> -o yaml > pod.yaml
kubect get deployment <deployment-name> -o yaml > deploy.yaml
kubect get service <service-name> -o yaml > service.yaml

## Export configuration without create resource (dry-run):

kubectl run <pod-name> --image=nginx --restart=Never dry-run -o yaml > pod.yaml 
kubectl run <deployment-name> --image=nginx --replicas=3 dry-run -o yaml > deploy.yaml

## Create a service:

kubectl create clusterip <service-name> --tcp=8080:80
kubectl create svc nodeport <service-name> --tcp=8080:80

## Expose an application:

kubectl expose pod <pod-name> --type=ClusterIp --name=<service-name> --port=80 
kubectl expose deploy <pod-name> --type=NodePort --name=<service-name> --port=80 --target-port=8080

## Shortcuts:

kubectl get rs (ReplicaSet)
kubectl describe cm (ConfigMap)
kubectl edit pv (PersistenceVolume)
kubectl delete pvc (PersistenceVolumeClaim)
kubectl create svc (Service)
kubectl get po (Pod)
kubectl describe no (Node)
kubectl get ep (Endpoint)
kubectl describe ing (Ingress)
kubectl edit ns (Namespace)
kubectl get netpol (NetworkPolicy)
kubectl edit sa (ServiceAccount)
kubectl delete cj (CronJob)
kubectl get hpa (HorizontalSclale)

## Labels:

kubectl label pod <pod-name> env=dev
kubectl label pod <pod-name> env=prod --overwrite

## Taints:

kubectl taint nodes <node-name> app_label=prod:NoSchedule

## Create configMap:

kubectl create cm <confi-map-name> --from-literal=keyValue=ValueABC

## Create secret:

kubectl create secret generic <secret-name> --from-literal=keyValue=ValueABC

For check the value generated:
echo -n 'ValueABC' | base64
echo -n 'VmFsdWVBQkM=' | base64 --decode

## Get logs:

kubectl logs -f <pod-name>
kubectl logs -f <pod-name> <container-name> (MultiPod Container)
kubectl logs -f <pod-name> > logs.log (Export the logs to file)

## Deployments commands:

kubectl scale deploy <deployment-name> --replicas=7
kubectl set-image deploy <deployment-name> container-name=nginx:v1.1.1
kubectl rollout history deploy <deployment-name>
kubectl rollout status deploy <deployment-name>
kubectl rollout undo deploy <deployment-name>
kubectl rollout undo deploy <deployment-name> --to-revision=2

## Execute command in the pod:

kubectl exec -it <pod-name> -- env (Print all environment variables)
kubectl exec -it <pod-name> -- /bin/sh -c 'echo test'
kubectl exec -it <pod-name> -- cat /etc/apt/log-test.log 

## Get infos about resources:

kubectl get po --selector=env=dev
kubectl get po -o wide (details in one line)
kubectl deploy --show-labels
kubectl get po -l env=prod (filter by label)

### References
- [dgkanatsios](https://github.com/dgkanatsios/CKAD-exercises)
