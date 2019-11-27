# 4. Exposing OpenShift applications

Once it has been verified that the application is up and running as instructed in the [previous lab (Lab 3)](../Lab3/README.md), the next step is to configure access for the application outside of the cluster. There are several ways to do this:

## Option 1: Port-forwarding

If you want to quickly access a port of a specific pod of your cluster, you can also use the oc `port-forward` command:

```
$ oc port-forward POD [LOCAL_PORT:]REMOTE_PORT
```

## Option 2: Node Port services

This is the cleanest way to access the applications outside of OpenShift environment both locally and publicly. This way essentially makes use of the cluster node's IPs and a port in between the range (30000-32767) and tells OpenShift to proxy to the underlying application via. the port. This is better than the other two solutions for several reasons: we don't have to worry about port clashes, this works for non HTTP based services and finally, does not require a public host name.

To expose our deployment via NodePort, we simply expose the deployment with a *NodePort* type and label it with name *nodejs-ex-nodeport*

```console
$ oc expose dc nodejs-ex --type=LoadBalancer --name=nodejs-ex-nodeport
service/nodejs-ex-ingress exposed
```

To see the NodePort created, we can run:

```console
$ oc get svc nodejs-ex-nodeport
```

We should then be able to access the application in the browser, there is no installed browser on this server. In order to access to application, we use the following command

```console
$ curl [External-IP]:[PORT]
```

## Option 3: Routes

For web applications, the most common way to expose it is by a route. A route exposes the service as a host name. You can do this by running the command providing you have a host name available:

```
$ oc expose svc/nodejs-ex
$ oc get routes
```

We should then be able to access the application in any browser !


Congratulations! You have completed all labs in this workshop! You have learnt how to:
- Create an OpenShift project
- Create an OpenShift application in various ways
- How to monitor the status of an application
- How to access and expose your application

For more information on how to navigate OpenShift, check the [OpenShift docs](https://docs.openshift.com/dedicated/3/welcome/index.html)
