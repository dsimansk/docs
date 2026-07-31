---
title: "v1.23 release"
linkTitle: "v1.23 release"
author: "[dsimansk](https://github.com/dsimansk)"
author handle: https://github.com/dsimansk
date: 2026-07-31
description: "Knative v1.23 Release Announcement"
type: "blog"
---

## Announcing Knative v1.23 Release

A new version of Knative is now available across multiple components.

Follow the instructions in [Installing Knative](https://knative.dev/docs/install/) to install the components you require.

## Highlights

- **Generic ephemeral volumes** in Serving, enabling dynamically provisioned pod-scoped storage
- **EndpointSlice migration** in Eventing, moving off deprecated v1 Endpoints
- **IPv6 support** for Serving autoscaler EndpointSlice creation
- **Security updates** across Kafka Broker dependencies and a RabbitMQ ingress shutdown race fix

Minimal supported version of Kubernetes is 1.34. See our [release schedule](https://github.com/knative/community/blob/main/mechanics/RELEASE-SCHEDULE.md) for details.

## Serving

**Release notes**: [Knative Serving 1.23](https://github.com/knative/serving/releases/tag/knative-v1.23.0)

This release adds ephemeral volume support, IPv6 networking improvements, and startup probe validation.

**Generic Ephemeral Volumes**

Serving now supports generic ephemeral volumes via `volumeClaimTemplate`, behind the `kubernetes.podspec-volumes-ephemeral` feature flag ([#16590](https://github.com/knative/serving/pull/16590) by [@jbunting](https://github.com/jbunting)). This allows workloads to request dynamically provisioned storage that is tied to the pod lifecycle without needing pre-created PVCs.

**Startup Probe Validation**

Validation has been added for startup probes in user and sidecar containers ([#16594](https://github.com/knative/serving/pull/16594) by [@thiagomedina](https://github.com/thiagomedina)), catching misconfigured probes at admission time rather than at runtime.

**IPv6 Support for Autoscaler**

The autoscaler EndpointSlice creation and activator port replacement now support IPv6 ([#16591](https://github.com/knative/serving/pull/16591) by [@linkvt](https://github.com/linkvt)), extending dual-stack networking support in Serving.

**Bug Fixes**

A fix was made to the ResponseRecorder hijack state tracking for WebSocket connections ([#16611](https://github.com/knative/serving/pull/16611) by [@immanuwell](https://github.com/immanuwell)).

---

## Eventing

**Release notes**: [Knative Eventing 1.23](https://github.com/knative/eventing/releases/tag/knative-v1.23.0)

This release focuses on reliability fixes and modernizing the networking layer.

**EndpointSlice Migration**

Eventing has migrated from deprecated v1 Endpoints to `discovery.k8s.io/v1` EndpointSlice ([#9032](https://github.com/knative/eventing/pull/9032) by [@creydr](https://github.com/creydr)), aligning with the Kubernetes deprecation timeline and improving scalability.

**Configurable klog Verbosity**

A `klog-verbosity` flag has been added to the `config-logging` ConfigMap ([#9035](https://github.com/knative/eventing/pull/9035) by [@Ankitsinghsisodya](https://github.com/Ankitsinghsisodya)), giving operators more control over log verbosity without redeploying.

**Bug Fixes**

- JobSink status Location paths for event metadata containing slashes have been fixed ([#9100](https://github.com/knative/eventing/pull/9100) by [@immanuwell](https://github.com/immanuwell))
- A goroutine and memory leak in `SubjectAndFiltersPass` has been fixed ([#9151](https://github.com/knative/eventing/pull/9151) by [@creydr](https://github.com/creydr))
- The vreplica spreading across `minReplicas` pods in the scheduler has been fixed ([#9157](https://github.com/knative/eventing/pull/9157) by [@creydr](https://github.com/creydr))

---

## Eventing Extensions

### Apache Kafka Broker

**Release notes**: [Kafka Broker 1.23](https://github.com/knative-extensions/eventing-kafka-broker/releases/tag/knative-v1.23.0)

This release brings improved error handling, security patches, and several data-plane fixes.

**Improved Error Handling**

Error handling has been improved across broker, channel, consumer group, and trigger reconcilers ([#4724](https://github.com/knative-extensions/eventing-kafka-broker/pull/4724) by [@chriscannon](https://github.com/chriscannon)), making it easier to diagnose reconciliation failures.

**Security Updates**

Netty, logback-core, vertx-core, and jackson-core have been updated to resolve known vulnerabilities ([#4744](https://github.com/knative-extensions/eventing-kafka-broker/pull/4744)).

**Bug Fixes**

- The KafkaSink reconciler cluster admin error has been fixed ([#4720](https://github.com/knative-extensions/eventing-kafka-broker/pull/4720) by [@chriscannon](https://github.com/chriscannon))
- A null check has been added to `ExactFilter` to handle missing `FilterAttributes` ([#4717](https://github.com/knative-extensions/eventing-kafka-broker/pull/4717) by [@Dominic-Stout-GA-i3](https://github.com/Dominic-Stout-GA-i3))
- The OpenTelemetry BOM ordering has been fixed to ensure the declared version takes precedence ([#4752](https://github.com/knative-extensions/eventing-kafka-broker/pull/4752) by [@creydr](https://github.com/creydr))
- A timer drift in the cache implementation has been fixed ([#4762](https://github.com/knative-extensions/eventing-kafka-broker/pull/4762) by [@dsimansk](https://github.com/dsimansk))

### RabbitMQ Broker and Source

**Release notes**: [RabbitMQ 1.23](https://github.com/knative-extensions/eventing-rabbitmq/releases/tag/knative-v1.23.0)

This release adds adapter customization and fixes an ingress shutdown race condition.

**Customizable Adapter Container Name**

The RabbitmqSource adapter container name can now be customized ([#1762](https://github.com/knative-extensions/eventing-rabbitmq/pull/1762) by [@vgaidarji](https://github.com/vgaidarji)).

**Ingress Shutdown Race Fix**

A race condition between RabbitMQ connection close and HTTP drain during ingress shutdown has been fixed ([#1760](https://github.com/knative-extensions/eventing-rabbitmq/pull/1760) by [@deadtrickster](https://github.com/deadtrickster)).

---

## Client

**Release notes**: [Client 1.23](https://github.com/knative/client/releases/tag/knative-v1.23.0)

A maintenance release with dependency updates and CI improvements.

---

## Operator

<!-- TODO: Operator 1.23 release pending -->

**Release notes**: [Operator 1.23](https://github.com/knative/operator/releases/tag/knative-v1.23.0)

TODO: Add operator release notes when the release is available.

---

## Thank you, contributors

Release Leads:

- [@dsimansk](https://github.com/dsimansk)

New Contributors 🎉:

- [@brucearctor](https://github.com/brucearctor)
- [@chriscannon](https://github.com/chriscannon)
- [@deadtrickster](https://github.com/deadtrickster)
- [@Dominic-Stout-GA-i3](https://github.com/Dominic-Stout-GA-i3)
- [@gouthamhusky](https://github.com/gouthamhusky)
- [@immanuwell](https://github.com/immanuwell)
- [@jahnavigajjala-3](https://github.com/jahnavigajjala-3)
- [@jbunting](https://github.com/jbunting)
- [@vgaidarji](https://github.com/vgaidarji)

---

## Learn more

Knative is an open source project that anyone in the [community](https://knative.dev/docs/community/) can use, improve, and enjoy. We'd love you to join us!

- [Knative docs](https://knative.dev/docs)
- [Quickstart tutorial](https://knative.dev/docs/getting-started)
- [Samples](https://knative.dev/docs/samples)
- [Knative working groups](https://github.com/knative/community/blob/main/working-groups/WORKING-GROUPS.md)
- [Knative User Mailing List](https://groups.google.com/forum/#!forum/knative-users)
- [Knative Development Mailing List](https://groups.google.com/forum/#!forum/knative-dev)
- Knative on Twitter [@KnativeProject](https://twitter.com/KnativeProject)
- Knative on [StackOverflow](https://stackoverflow.com/questions/tagged/knative)
- Knative [Slack](https://slack.cncf.io)
- Knative on [YouTube](https://www.youtube.com/channel/UCq7cipu-A1UHOkZ9fls1N8A)
