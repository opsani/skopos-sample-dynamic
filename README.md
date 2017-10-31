### Skopos Sample Dynamic

#### Sample application that demonstrates several Skopos capabilities:
 - **dynamic component deploy** -  use a single model for multiple environments and control the set of components that will get deployed by changing variables. This allows for different environments to share the same model, even if they run don't run the all of the services defined in the model, or they run different versions/number of replicas
- **quality gate probes** - verify a deployed/upgraded service is operating as expected, or trigger a rollback
- **remote descriptors** - load model and environment files from a remote URL

#### Files
 - [*model.yaml*](model.yaml) - Model file that describes the full application. It defines a quality gate probe for the 'result' component that waits until the service starts serving '200 OK' responses to a given request
 - [*env-common.yaml*](env-common.yaml) - Common environment definition. Specifies the default version of all component as well as the default replica count (zero). It provides another way of specifying quality probes - useful if you want to define the same health check for multiple (or all) components
 - [*env-all.yaml*](env-all.yaml) - Environment definition that sets the replica count for components when deploying the full application
 - [*env-all-upgrade.yaml*](env-all-upgrade.yaml) - Environment definition that specifies different component versions for 'vote' and 'result' (i.e. trigger an upgrade)
 - [*env-vote-only.yaml*](env-vote-only.yaml) - Environment definition that sets the replica count for components when deploying just a part of the application ('vote' + 'result')

#### Example commands (using descriptors from a remote URL)

```
# Deploy full application
skopos run -project demo -env https://raw.githubusercontent.com/opsani/skopos-sample-dynamic/master/env-common.yaml -env https://raw.githubusercontent.com/opsani/skopos-sample-dynamic/master/env-all.yaml https://raw.githubusercontent.com/opsani/skopos-sample-dynamic/master/model.yaml

# Deploy the full application with upgraded version of 'vote' and 'result'
skopos run -project demo -env https://raw.githubusercontent.com/opsani/skopos-sample-dynamic/master/env-common.yaml -env https://raw.githubusercontent.com/opsani/skopos-sample-dynamic/master/env-all-upgrade.yaml https://raw.githubusercontent.com/opsani/skopos-sample-dynamic/master/model.yaml

# Deploy just the voting part of the application - 'vote' and 'redis', all
# other components are disabled (i.e. have zero replicas)
skopos run -project demo -env https://raw.githubusercontent.com/opsani/skopos-sample-dynamic/master/env-common.yaml -env https://raw.githubusercontent.com/opsani/skopos-sample-dynamic/master/env-vote-only.yaml https://raw.githubusercontent.com/opsani/skopos-sample-dynamic/master/model.yaml
```

#### Doc links

 - deployment verification (a.k.a quality gates): http://doc.opsani.com/skopos/edge/VERIFY-GUIDE/#deployment-verification
 - quality gates in env file: http://doc.opsani.com/skopos/edge/VERIFY-GUIDE/#quality-gates-in-environment-descriptors
 - http probe: https://github.com/opsani/probe-http

