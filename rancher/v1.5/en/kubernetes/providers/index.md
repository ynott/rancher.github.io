---
title: Kubernetes Cloud Providers
layout: rancher-default-v1.5
version: v1.5
lang: en
---

## Kubernetes - Cloud Providers
---

In Kubernetes, there is a concept of [cloud providers](https://kubernetes.io/docs/getting-started-guides/scratch/#cloud-provider), which is a module which provides an interface for managing load balancers, nodes (i.e. hosts) and networking routes.

Currently, Rancher supports two cloud providers when [configuring Kubernetes]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/kubernetes/#configuring-kubernetes). You can select which cloud provider to use.

### Rancher

  * **Nodes:** Supports any [hosts]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/hosts/) that can be added in Rancher.
  * **Load Balancers:** Launches Rancher's load balancer, which uses HAproxy and the `rancher/lb-service-haproxy` image, as a Load Balancer service. By default, the load balancer will round robin traffic to the pods.

By default, the orchestration for Kubernetes is set to `rancher`.

### AWS

  * **Nodes:** Supports only AWS hosts added either as a [custom host]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/hosts/custom/) or [through the Rancher UI]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/hosts/amazon/).
  * **Load Balancers:** Launches an AWS Elastic Load Balancer (ELB) as a Load Balancer service. You can still create Rancher load balancers by using an [ingress]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/kubernetes/ingress/).
  * **Persistent Volumes**: Ability to use AWS Elastic Block Stores (EBS) for [persistent volumes]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/kubernetes/ingress/storage/).

#### Adding Hosts

After Kubernetes has been configured to run with an `aws` cloud provider, any hosts added into the environment will need to be an AWS EC2 instance.

In order to use Elastic Load Balancers (ELBs) and EBS with Kubernetes, the host will need to have the an IAM role with appropriate access.

##### Example Policy for IAM Role:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ec2:Describe*",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "ec2:AttachVolume",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "ec2:DetachVolume",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": ["ec2:*"],
      "Resource": ["*"]
    },
    {
      "Effect": "Allow",
      "Action": ["elasticloadbalancing:*"],
      "Resource": ["*"]
    },
  ]
}
```

#### Elastic Load Balancer (ELB) as a Kubernetes service

After [configuring Kubernetes]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/kubernetes/#configuring-kubernetes) to use `aws` as a cloud provider and ensuring the host has the appropriate IAM policy for ELB, you can start creating load balancers.  

##### Example `lb.yml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-lb
  labels:
    app: nginx
spec:
  type: LoadBalancer
  selector:
    app: nginx
  ports:
  - name: http
    port: 80
    protocol: TCP
```

Using `kubectl`, let's launch our load balancer service into Kubernetes. Remember, you can either [configure `kubectl` for your local machine]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/kubernetes/#kubectl) or you can use the shell in the UI under **Kubernetes** -> **kubectl**.

```bash
$ kubectl create -f lb.yml
service "nginx-lb" created
$ kubectl describe services nginx-lb
Name:			nginx-lb
Namespace:		default
Labels:			app=nginx
Selector:		name=nginx
Type:			LoadBalancer
IP:			10.43.137.5
LoadBalancer Ingress:	a4c7d4290f48011e690470275ac52fef-1158549671.us-west-2.elb.amazonaws.com
Port:			http	80/TCP
NodePort:		http	32166/TCP
Endpoints:		<none>
Session Affinity:	None
Events:
  FirstSeen	LastSeen	Count	From			SubObjectPath	Type		Reason			Message
  ---------	--------	-----	----			-------------	--------	------			-------
  17s		17s		1	{service-controller }			Normal		CreatingLoadBalancer	Creating load balancer
  14s		14s		1	{service-controller }			Normal		CreatedLoadBalancer	Created load balancer
```

#### Using EBS Volumes

After configuring Kubernetes to use `aws` as a cloud provider and ensuring the host has the appropriate IAM policy for EBS, you can start [using EBS volumes]({{site.baseurl}}/rancher/{{page.version}}/{{page.lang}}/kubernetes/storage/#persistent-volumes---ebs).
