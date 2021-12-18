### Cartographer supply chain that runs workloads using a Kubernetes deployment

This has been tested with [Tanzu Application Platform](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/index.html) Beta 4.

Create the supply chain:

* `kubectl create -f deployment-config-template.yaml`
* `kubectl create -f source-to-url-deployment.yaml`

Before using it, update the RBAC rules for your [developer namespace](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/0.4/tap/GUID-install-components.html#set-up-developer-namespaces-to-use-installed-packages-43) by editing the default role (change the namespace if your developer namespace isn't default):
`kubectl edit role default -n default`

And add the following to the list of rules:
```yaml
  - apiGroups:
    - ""
    resources:
    - deployments
    verbs:
    - '*'
```

Now you're ready to create a workload that uses the supply chain (requires the [tanzu CLI](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/0.3/tap-0-3/GUID-install-general.html#cli-and-plugin)):

* `tanzu apps workload create dotnet-mvc --git-repo https://github.com/fjb4/tanzu-dotnet-web-app --git-branch master --type web-deployment`

Note that using the supply chain requires you to specify a workload type of "web-deployment".
