# DevOps Technical Assessment

  

## Helm charts

The repository conatins my attempt to solve given issue of creating deployment of 2 services to the K8S cluster.

  

### Layout

`backend-chart` contains helm chart for backend service, and `frontend-chart` for frontend service, additional files in the root directory are values for used charts (additionaly chart for ingress controller and DB was used). `helmfile.yaml` is a declaration of all helm releases that need to be deployed.

  

### Deployment

`helmfile` offeres declarative attempt to orginize helm releases (for more information read: https://helmfile.readthedocs.io/en/latest)

To deploy simply run:

```bash

helmfile  apply  -i

```

Helmfile will take care of everything else.

  

## Explanation

### Choice of database and ingress controller

This is the first time I've deployed DB solution to kubernetes, I'm more used to using managed solutions from public clouds. I decided to go with postgres, as it's well known, widely supported solution for tradicional transactional DBs. I'm currently feeling the most comfortable with it. Regarding Ingress controller I've decided to go with nginx. There are multiple reasons for that, I wanted to choose simple and easy to configure solution, I've used nginx in the past so I know it, I didn't want to go into more advanced solutions like something from istio or cilium, as I haven't had in plan to use other functionalities.

### Secrets management

The solution proposed by me is not production ready, as a final step I'd just add sops (see: https://github.com/getsops/sops). I like concept of storing secrets in the managed way in cloud, the idea of directly grabbing them or using external-secrets (https://external-secrets.io/latest/) is quite interesting to me (I like the similarities to cert-menager: https://cert-manager.io/). Right now secrets are just stored as kubernetes secrets with no additional protection (their initial values are even here in repo).

### High availability for the pods

I've added automatic horizontal scaling with minimum replica number set to 2 (one pod might always broke). What may be done in the future is to tune the autoscaler policies to keep the pods on different nodes. Additionally number of nodes should be definitely bigger than one.

### Architectural choices you made during the task

I was a little bit sad that I was not able to write terraform :') to adjust the solution for cloud scenerio. Another think is that I think that Cilium is a good solution for controlling traffic in the cluster (the scale was not big enough for using it here).

### Include any testing or verification steps you'd recommend post-deployment

No testing (different than manual playing with what I created) was done, I'm really curious what can be done here. I can imagine that some sort of automation here might be very helpful but I don't know any solution or patterns in that area.