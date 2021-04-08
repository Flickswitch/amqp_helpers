# AMQP Helpers

Non opinioned [AMQP](https://github.com/pma/amqp) helpers that implement common
utilities for several [AMQP](https://www.amqp.org/)/[RabbitMQ](https://www.rabbitmq.com/)
scenarios.

## Installation

To use _AMQPHelpers_ you need [AMQP](https://github.com/pma/amqp). You can
install both by adding `amqp` and `amqp_helpers` to your list of dependencies in
`mix.exs`:

```elixir
def deps do
  [
    {:amqp, "~> 2.1"},
    {:amqp_helpers, "~> 0.1"}
  ]
end
```

## Motivation

This library provides several wrappers and utilities around some common use
cases of _AMQP_. It provides a simple interface, following common _OTP_ patterns
and [Elixir library guidelines](https://hexdocs.pm/elixir/master/library-guidelines.html)
to avoid any opinionated interface. Other generalist abstractions can be built
on top of _AMQPHelpers_ to provide ergonomics or any other feature not strictly
tied to _AMQP_.

Right now, the utilities built in this library are suited for uses that try to
optimize _AMQP_ for high throughput and for scenarios that require data safety
at any cost.

### Comparisons With Other Libraries

**TODO**

## AMQP Good Practices

**TODO (two connections, mox and adapters)**

## User Case Scenarios

Two _AMQP_ use cases are covered in _AMQP Helpers_ right now:

- A performance-intensive scenario in which a high throughput message delivery
  rate is desired. Trade-offs in reliability are acceptable.

- A reliable scenario in which data safety is a must, even if performance is
  compromised.

There are some other features, like _High Availability_, _Observability_,
_Exclusivity_, etc. that can be achieved in both scenarios but are not
explicitly covered here.

### High Throughput

To achieve the best performance in terms of message delivery some trade-off must
be done, which usually impacts the reliability and/or coherence of the system.

Durability should be disabled. In other words, messages will not be persisted,
so messages could be lost in a broker outage scenario. This can be achieved by
declaring queues as non-durable and publishing messages with `persistent` set to
`false`.

Acknowledges, from any communication direction, should be disabled. This means
that the publisher should not confirm deliveries, and consumers should not
acknowledge messages. Messages could be lost on the flight because of network or
edge issues. Publisher confirms are not enabled by default, so nothing has to be
done here in terms of publishing. Consuming requires disabling acknowledging,
which can be done by setting the `no_ack` flag on.

The `AMQPHelpers.HighThroughput` module provides functions that enforce these
requirements for publishing and consuming. These are simple wrappers around
_AMQP_ library functions. They add very little aside from being explicit about
a feature of some scenario (performance intensive).

**TODO (AMQP schema properties: max-length, disable lazy queues, diable HA)**

### Reliability

## Testing

**TODO**

## References

- [CloudAMQP - RabbitMQ Best Practices for High Availability](https://www.cloudamqp.com/blog/part3-rabbitmq-best-practice-for-high-availability.html)
- [CloudAMQP - RabbitMQ Best Practices for High Performance](https://www.cloudamqp.com/blog/part2-rabbitmq-best-practice-for-high-performance.html)
- [CloudAMQP - RabbitMQ Best Practices](https://www.cloudamqp.com/blog/part1-rabbitmq-best-practice.html)
- [RabbitMQ Docs - Reliability Guide](https://www.rabbitmq.com/reliability.html)
- [RabbitMQ in Depth](https://www.manning.com/books/rabbitmq-in-depth)
