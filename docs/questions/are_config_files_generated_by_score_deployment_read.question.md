---
title: "Are the configuration files generated by Score deployment-ready?"
---

This depends on the complexity of your set-up. The "Hello World" guides for [score-compose](https://github.com/score-spec/score-compose/tree/main/examples) and [score-helm](https://github.com/score-spec/score-helm/tree/main/examples) allows you to directly deploy a workload. In more complex use cases, however, it is likely that the configuration generated by Score is combined with additional configuration provided by a platform or operations team. This allows for a clean separation of concerns between the developer owned workload related configuration and operations owned platform - and infrastructure related configuration.

To understand what this means in practice, the following needs to be considered:

Score only describes workload level properties. This means, if you’re deploying to an environment that runs on a system such as Kubernetes, you’ll likely have additional platforms and environment-specific configuration in place.

For example: You might want to set up `namespace.yaml` and `ingress.yaml` files for the deployments. Score assumes that any configuration outside the workload (and developer) scope is defined and managed externally, for example by an operations team. This allows you to combine the configuration generated by Score with more advanced infrastructure configuration if needed and doesn’t limit teams in leveraging the full potential of their platforms.

The Score Specification is defined in an environment-agnostic way. This means environment-specific parameters (such as variable values and secrets) need to be injected in the target environment.

For example: In your development environment, a database connection string such as: `postgres://${postgres.username}:{postgres.password}@${postgres.host}:${postgres.port}/${postgres.name}` might be resolved to `postgresql://admin:password@group_db_host.example.com:1521/db.example.com` while in staging and production you’ll want to have different credentials to be inserted. How, and from where these values are injected, is up to the platform in the target environment.

Score allows users to define resource dependencies for their service. It does not declare by whom, when and how the resource should be provisioned and allocated in the target environment. It is up to the Score Implementation to resolve the resource by name, type, or any other meta information available. For example: a dependency on a Postgres database could be resolved by a Docker image, a mock service, Terraform, a custom provisioning script or even a manual action - it is totally up to the platform (team).

If you’d like to learn more about the philosophy behind our way of separating concerns, I recommend reading the article [Why we advocate for workload-centric over infrastructure-centric development](https://score.dev/blog/workload-centric-over-infrastructure-centric-development).