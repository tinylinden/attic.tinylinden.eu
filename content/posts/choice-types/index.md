---
title: "Choice Types"
date: 2023-08-17T14:00:00+02:00
draft: true
tags: ["ddd","fp","kotlin"]
summary: "Capturing business rules in type system"
---

## Capturing Rules in the Type System

A sequence of characters (aka. `String`) can represent valid or invalid
[IBAN][iban], these are the only available, and mutually exclusive options. 
Value is valid from its inception, or was never valid at all. So type used
for holding IBAN values should _scream_ about that.

```kotlin
sealed interface Iban

@JvmInline
value class ValidIban(
    val raw: String
) : Iban

data class InvalidIban(
    val raw: String,
    val reason: String
) : Iban
```

With above definitions we can harness the type system to ensure code 
corectness. For example -- there will be no way to place money transfer 
order to or from invalid account, if type representing such order will 
require valid values. And that requirement is explicit - the value must
be of type `ValidIban`.

```kotlin
data class MoneyTransferOrder(
    val debit: ValidIban,
    val credit: ValidIban,
    // more boring details necessary to transfer money
)
```

No more checking if some flag was set, to process or reject such order.
All we have to do is prevent creation of `ValidIban` holding actually 
incorrect value.

`InvalidIban` case may seem slightly useless -- who needs to keep info
about invalid value? But wait, what if such value comes from an event
(you know, something that already happened), and data source for event 
was OCR of handwritten refund form? 

{{< figure
    src="handwritten.png"
    alt="Handwritten IBAN"
    caption="Is that `IE12B0F1...` or `IE12BOFI...`?" 
>}}

## Forcing Correctness

We can show optimistic attitude and assume that all incoming values are
valid, and always try to create `ValidIban`. At the same time inside
initializer block we can enforce validity requirements, so `ValidIban`
holding actually incorrect value could not happen.

```kotlin
@JvmInline
value class ValidIban(val raw: String) : Iban {
    init {
        require(raw satisfies structureCond)
        require(raw satisfies ::checkDigitCond)
    }
}

// let's define conditions
val structureCond: Condition<String> = { 
    TODO("Verify structure") 
}

fun checkDigitCond(raw: String): Boolean = 
    TODO("Verify check digit")

// and add some sugar required for that dsl-like syntax
typealias Condition<T> = (T) -> Boolean
infix fun <T> T.satisfies(cond: Condition<T>): Boolean = cond(this)
```

## Getting Rid of Side Effects

Incorrect value creation is impossible, but now not only we need to be
aware of the side effect in form of `IllegalArgumentException`, but also
we need to handle it every time. Solution to this inconvenience could not
be simpler than _total factory function_. And by _total function_ I mean 
one that gives a valid return value for every combination of arguments.

```kotlin
fun iban(raw: String): Iban =
    try {
        ValidIban(raw)
    } catch (ex: IllegalArgumentException) {
        InvalidIban(raw, ex.message ?: "Unknown")
    }
```

## Gluing Everything Together

When types specifies exactly what to expect, really simple and readable
code is possible.

```kotlin
fun onRefundAccepted(event: RefundAcceptedEvent) {
    val iban = iban(event.accountNo)

    when (iban) {
        is ValidIban -> TODO("Place money transfer order")
        is InvalidIban -> TODO("Notify customer about problem")
    }
}
```

[iban]: https://en.wikipedia.org/wiki/International_Bank_Account_Number
[ddd-book]: https://pragprog.com/titles/swdddf/domain-modeling-made-functional/
