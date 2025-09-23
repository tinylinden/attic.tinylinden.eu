---
title: "Mocked Schema Registry"
date: 2025-08-12T19:00:00+02:00
tags: ["testing","avro","kafka","spring","kotlin"]
summary: "How complicated can it be to add Schema Registry to integration test"
draft: true
---

Just wanted to check how complicated can it be to add [Schema Registry][schema-registry]
to `@SpringBootTest` to be sure that configuration is working, and [Spring Cloud Stream][spring-cloud-stream]
consumer using [Kafka Binder][kafka-binder] can deserialize data using AVRO schema?

The only restriction I imposed on myself was the inability to use [Testcontainers][testcontainers].
That's just how it is, let's not go into the reasons why. Or maybe it's just the subject for another
story.

## Simplified Anatomy of Schema Registry

To be described...

{{< figure
    src="simplified-anatomy-of-schema-registry.png"
>}}



[schema-registry]: https://docs.confluent.io/platform/current/schema-registry/index.html
[spring-cloud-stream]: https://spring.io/projects/spring-cloud-stream
[kafka-binder]: https://docs.spring.io/spring-cloud-stream/reference/kafka/spring-cloud-stream-binder-kafka.html
[testcontainers]: https://testcontainers.com/
