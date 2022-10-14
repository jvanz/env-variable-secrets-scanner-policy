# Kubewarden policy env-variable-secrets-scanner-policy

## Description

This policy will reject pods that contain a secret in an environment variable in any container. 
This policy uses [rusty hog](https://github.com/newrelic/rusty-hog) as a secret scanner. It looks for
all the secrets in the rusty-hog [default_rules.json](https://github.com/newrelic/rusty-hog/blob/v1.0.11/src/default_rules.json).

Some secrets that will be detected are: Slack token, RSA private key, SSH private key, Facebook tokens, AWS tokens, Google API Keys, 
New Relic Keys, ...  For a complete list visit [rusty-hog website](https://github.com/newrelic/rusty-hog)

The policy can either target `Pods`, or [workload
resources](https://kubernetes.io/docs/concepts/workloads/) (`Deployments`,
`ReplicaSets`, `DaemonSets`, `ReplicationControllers`, `Jobs`, `CronJobs`) by
setting the policy's `spec.rules` accordingly.

Both have trade-offs:
* Policy targets Pods: Different kind of resources (be them native or CRDs) can
  create Pods. By having the policy target Pods, we guarantee that all the Pods
  are going to be compliant, even those created from CRDs.
  However, this could lead to confusion among users, as high level Kubernetes
  resources would be successfully created, but they would stay in a non
  reconciled state. Example: a Deployment creating a non-compliant Pod would be
  created, but it would never have all its replicas running.
* Policy targets workload resources (e.g: Deployment): the policy inspect higher
  order resource (e.g. Deployment): users will get immediate feedback about
  rejections.
  However, non compliant pods created by another high level resource (be it
  native to Kubernetes, or a CRD), may not get rejected.

## Settings

This policy has no configurable settings.

## License

```
Copyright (C) 2021 raulcabello <raul.cabello@suse.com>

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
