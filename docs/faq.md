# Frequently Asked Questions

#### Is Cirrus CI a delivery platform?

Cirrus CI is not positioned as a delivery platform but can be used as one for many general use cases by having 
[Dependencies](guide/writing-tasks.md#dependencies) between tasks and using [Conditional Task Execution](guide/writing-tasks.md#conditional-task-execution):

```yaml
lint_task:
  script: yarn run lint

test_task:
  script: yarn run test

publish_task:
  only_if: $BRANCH == 'master'
  depends_on: 
    - test
    - lint
  script: yarn run publish
```

#### CI agent stopped responding!

It means that Cirrus CI haven't heard from the agent for quite some time. In 99.999% of the cases 
it happens because of two reasons:

1. Your task was executing on [Community Cluster](guide/supported-computing-services.md#community-cluster). Community Cluster 
is backed by Google Cloud's [Preemptible VMs](https://cloud.google.com/preemptible-vms/) for cost efficiency reasons and
Google Cloud preempted back a VM your task was executing on. Cirrus CI is trying to minimize possibility of such cases 
by constantly rotating VMs before Google Cloud preempts them, but there is still chance of such inconvenience.

2. Your CI task used too much memory which led to a crash of a VM or a container.

#### Instance failed to start!

It means that Cirrus CI have made a successful API call to a [computing service](/guide/supported-computing-services.md) 
to allocate resources. But a requested resource wasn't created. 

If it happened for an OSS project, please contact [support](/support.md) immediately. Otherwise check your cloud console first 
and then contact [support](/support.md) if it's still not clear what happened. 

#### Only GitHub Support?

At the moment Cirrus CI only supports GitHub via a [GitHub Application](https://github.com/apps/cirrus-ci). We are planning
to [support BitBucket](https://github.com/cirruslabs/cirrus-ci-docs/issues/9) next. 

#### Any discounts?

Cirrus CI itself doesn't provide any discounts except [Community Cluster](/guide/supported-computing-services.md#community-cluster) 
which is free for open source projects. But since Cirrus CI delegates execution of builds to different computing services,
it means that discounts from your cloud provider will be applied to Cirrus CI builds.
