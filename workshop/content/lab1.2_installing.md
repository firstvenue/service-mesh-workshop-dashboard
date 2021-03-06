# Setup

You will conduct these labs in an OpenShift cluster.  First, test you have access to your cluster via console and CLI.

## OpenShift

<blockquote>
<i class="fa fa-desktop"></i> If you check the Console tab in your dashboard, you should see the following:
</blockquote>

<img src="images/openshift-welcome.png" width="1024"><br/>
 *OpenShift Welcome*

<br>

You will use the OpenShift `oc` CLI  to execute commands for the majority of this lab.  


<blockquote>
<i class="fa fa-terminal"></i> You should already be logged in to your cluster in your web terminal.
</blockquote>

Switch to the **Terminal** tab, and try running:

```execute
oc whoami
```
*You can click the play button in the top right corner of the code block to automatically execute the command for you.*

<br>

The instructor will have preconfigured your projects for you.

<blockquote>
<i class="fa fa-terminal"></i> List your projects:
</blockquote>

```execute
oc projects
```

You should see two projects: your user project (e.g. '%username%') and 'istio-system'.  

<br>

<blockquote>
<i class="fa fa-terminal"></i> Switch to your user project.  For example:
</blockquote>

```execute
oc project %username%
```

<br>

Let's take a look at the project.

<blockquote>
<i class="fa fa-terminal"></i> List the pods in the project:
</blockquote>

```execute
oc get pods
```

Output (sample):

```
NAME                                       READY   STATUS    RESTARTS   AGE
istio-demogateway-user1-xxxxxxxxxx-xxxxx   1/1     Running   0          2m41s
keycloak-operator-xxxxxxxxx-xxxxx          1/1     Running   0          15h
```

The gateway is a load balancer dedicated to your project.  You will configure this load balancer in the next lab.  The keycloak operator will be used in the security labs.

<br>

Finally, set the project name variable.

<blockquote>
<i class="fa fa-terminal"></i>
Run this command:
</blockquote>

```execute
PROJECT_NAME=$(oc project -q)
```

<br>

## Application Code
Next we need a local copy of our application code.

<blockquote>
<i class="fa fa-terminal"></i> Clone the repository:
</blockquote>

```execute
git clone https://github.com/RedHatGov/openshift-microservices.git
```

<blockquote>
<i class="fa fa-terminal"></i> Checkout the workshop-stable branch:
</blockquote>

```execute
cd openshift-microservices && git checkout workshop-stable
```

<blockquote>
<i class="fa fa-terminal"></i>
Navigate to the workshop directory:
</blockquote>

```execute
cd deployment/workshop
```

<br>

## Istio
Istio should have been installed in the cluster by the instructor.  Let's make sure Istio is running in the cluster.  You will only have view access to the Istio project.

<blockquote>
<i class="fa fa-terminal"></i>
List all the Istio components:
</blockquote>

```execute
oc get pods -n istio-system
```

Output:

```
NAME                                      READY   STATUS    RESTARTS   AGE
grafana-xxxxxxxxx-xxxxx                  2/2     Running   0          17m
istio-citadel-xxxxxxxxx-xxxxx            1/1     Running   0          20m
istio-egressgateway-xxxxxxxx-xxxxx       1/1     Running   0          17m
istio-galley-xxxxxxxx-xxxxx              1/1     Running   0          19m
istio-ingressgateway-xxxxxxxxx-xxxxx     1/1     Running   0          17m
istio-pilot-xxxxxxxxx-xxxxx              2/2     Running   0          18m
istio-policy-xxxxxxxxx-xxxxx             2/2     Running   0          19m
istio-sidecar-injector-xxxxxxxxx-xxxxx   1/1     Running   0          17m
istio-telemetry-xxxxxxxxx-xxxxx          2/2     Running   0          19m
jaeger-xxxxxxxxx-xxxxx                   2/2     Running   0          19m
kiali-xxxxxxxxx-xxxxx                    1/1     Running   0          16m
prometheus-xxxxxxxxx-xxxxx               2/2     Running   0          19m
```

The primary control plane components are [Pilot][1], [Mixer][2], and [Citadel][3].  Pilot handles traffic management.  Mixer handles policy and telemetry.  Citadel handles security.

[1]: https://istio.io/docs/concepts/traffic-management/
[2]: https://istio.io/docs/concepts/observability/
[3]: https://istio.io/docs/concepts/security/
