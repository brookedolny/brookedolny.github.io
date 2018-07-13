---
layout: post
title:  "Introduction to Fields, Part 2"
description: An introduction to finite fields.
date:   2018-07-12 18:00:00 -0400
categories:
tags: cryptography encryption math
---

This post is an extension of [last week's post](/2018/07/10/introduction-to-fields.html) on basic field theory.
If you haven't read it yet, or aren't familiar with fields, I recommend taking a look at it before
reading this one.

## Review

Recall the definition of a field from last week:

**Definition:** We call a set $$ \mathbb{F} $$ a **field** if there are operators $$ + $$
(commonly called **addition**) and $$ \times $$ (commonly called **multiplication**)
such that all of the following properties are true:

1. $$ + $$ is **commutative**: $$ \quad \forall x, y\in\mathbb{F}, \; x + y = y + x $$   
2. $$ + $$ is **associative**: $$ \quad  \forall x, y, z\in\mathbb{F}, \; (x + y) + z = x + (y + z) $$    
3. existence of an **additive identity**: $$ \quad \exists 0 \in\mathbb{F} $$ such that $$ \forall x\in\mathbb{F}, \; x + 0 = 0 + x = x $$
4. existence of an **additive inverse**: $$ \quad \forall x \in\mathbb{F}, \; \exists (-x) \in \mathbb{F} $$
   such that $$ x + (-x) = -x + x = 0 $$
5. $$ \times $$ is **commutative**: $$ \quad \forall x, y\in\mathbb{F}, \; x \times y = y \times x $$    
6. $$ \times $$ is **associative**: $$ \quad \forall x, y, z\in\mathbb{F}, \; (x \times y) \times z = x \times (y \times z) $$    
7. **distributivity** of $$ \times $$ over $$ + $$: $$ \quad \forall x, y, z \in \mathbb{F}, \; x \times (y + z) = x \times y + x \times z $$
8. existence of a **multiplicative identity**: $$ \quad \exists 1 \in\mathbb{F} $$ such that
   $$ \forall x\in\mathbb{F}, \; x \neq 0, \; x \times 1 = 1 \times x = x $$
9. existence of an **multiplicative inverse**: $$ \quad \forall x \in\mathbb{F}, \; x \neq 0, \; \exists x^{-1} \in\mathbb{F} $$
   such that $$ x \times x^{-1} = 1 $$

Where $$ x \times y $$ is often denoted as $$ x \cdot y $$, or $$ xy $$.

I also mentioned that the real numbers, $$ \mathbb{R} $$ and the rational numbers,
$$ \mathbb{Q} $$ are both fields, but the integers, $$ \mathbb{Z} $$ are not (because they do not
satisfy property **9** in the above definition).

Fantastic! So, with the review out of the way, lets get into this weeks content.

## Extended Euclidean Algorithm

You may or may not have heard of the **Euclidean Algorithm**. This algorithm is a
method of computing the **Greatest Common Divisor (gcd)** of two integers.

**Definition:** the **Greatest Common Divisor (gcd)** of $$ a, b \in \mathbb{Z} $$ is
equal to $$ d $$ if $$ d \mid a $$, $$ d \mid b $$ and if $$ \exists c \in \mathbb{Z} $$, then
$$ c \leq d $$.

This is often denoted as $$ \textrm{gcd}(a, b) = d $$, and $$ x \mid y $$ is read as "**x divides y**"

The above definition is just a long-winded way of saying that $$ \textrm{gcd}(a, b) = d $$ if
$$ d $$ is the largest integer that divdes both $$ a $$ and $$ b $$. Also, note that $$ \textrm{gcd}(x, 0) = x $$.

Okay cool. So what's the Euclidean algorithm? Well...

**Theorem:** $$ \textrm{gcd}(a, b) = \textrm{gcd}(b, r)$$ where  $$ r $$ is the remainder
when $$ a $$ is divided by $$ b $$.

This is ultra nice because it's really easy to write a program to calculate the gcd of
$$ a $$ and $$ b $$:

{% highlight c %}
int gcd(int a, int b) {
    if (b == 0) {
        return a;
    }
    return gcd(b, a % b);
}
{% endhighlight %}

This (obviously) requires that $$ a \gt b $$, but that's fine. This
