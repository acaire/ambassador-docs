import Alert from '@material-ui/lab/Alert';

# Upgrade $productName$ 2.1.0 or 2.1.1 to $productName$ $version$ (Helm)

<Alert severity="info">
  This guide covers migrating from $productName$ 2.1.0 or 2.1.1 to $productName$ $version$. If
  this is not your <b>exact</b> situation, see the <a href="../../../../migration-matrix">migration
  matrix</a>.
</Alert>

<Alert severity="warning">
  This guide is written for upgrading an installation made using Helm.
  If you did not originally install with Helm, see the <a href="../../../yaml/edge-stack-2.1/edge-stack-2.1">Helm-based
  upgrade instructions</a>.
</Alert>

Since $productName$'s configuration is entirely stored in Kubernetes resources, upgrading between minor
versions is straightforward.

Migration is a two-step process:

1. **Install new CRDs.**

   Before installing $productName$ $version$ itself, you need to update the CRDs in
   your cluster; Helm will not do this for you. This is mandatory during any upgrade of $productName$.

   ```
   kubectl apply -f https://app.getambassador.io/yaml/edge-stack/$version$/aes-crds.yaml
   kubectl wait --timeout=90s --for=condition=available deployment emissary-apiext -n emissary-system
   ```

   <Alert severity="info">
     $productName$ $version$ includes a Deployment in the `emissary-system` namespace
     called <code>emissary-apiext</code>. This is the APIserver extension
     that supports converting $productName$ CRDs between <code>getambassador.io/v2</code>
     and <code>getambassador.io/v3alpha1</code>. This Deployment needs to be running at
     all times.
   </Alert>

   <Alert severity="warning">
     If the <code>emissary-apiext</code> Deployment's Pods all stop running,
     you will not be able to use <code>getambassador.io/v3alpha1</code> CRDs until restarting
     the <code>emissary-apiext</code> Deployment.
   </Alert>

   <Alert severity="warning">
    There is a known issue with the <code>emissary-apiext</code> service that impacts all $productName$ 2.x and 3.x users. Specifically, the TLS certificate used by apiext expires one year after creation and does not auto-renew. All users who are running $productName$/$OSSproductName$ 2.x or 3.x with the apiext service should proactively renew their certificate as soon as practical by running <code>kubectl delete --all secrets --namespace=emissary-system</code> to delete the existing certificate, and then restart the <code>emissary-apiext</code> deployment with <code>kubectl rollout restart deploy/emissary-apiext -n emissary-system</code>.
    This will create a new certificate with a one year expiration. We will issue a software patch to address this issue well before the one year expiration. Note that certificate renewal will not cause any downtime.
   </Alert>

2. **Install $productName$ $version$.**

   After installing the new CRDs, use Helm to install $productName$ $version$. This will install
   in the `$productNamespace$` namespace. If necessary for your installation (e.g. if you were
   running with `AMBASSADOR_SINGLE_NAMESPACE` set), you can choose a different namespace.

      ```bash
      helm upgrade -n $productNamespace$ \
         $productHelmName$ datawire/$productHelmName$ && \
      kubectl rollout status  -n $productNamespace$ deployment/$productDeploymentName$ -w
      ```

   <Alert severity="warning">
     You must use the <a href="https://github.com/datawire/edge-stack/"><code>$productHelmName$</code> Helm chart</a> to install $productName$ 2.X.
     Do not use the <a href="https://github.com/emissary-ingress/emissary/tree/release/v1.14/charts/ambassador"><code>ambassador</code> Helm chart</a>.
   </Alert>
