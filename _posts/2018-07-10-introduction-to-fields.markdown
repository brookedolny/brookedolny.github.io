---
layout: post
title:  "Introduction to Fields"
description: An simple introduction to basic field theory.
date:   2018-07-10 17:48:00 -0400
categories:
tags: cryptography encryption math
---

Field theory forms the basis for all forms of encryption systems where decryption is desired.
RSA, ECC, and AES (which are acronyms you may or may not have heard of)
are just a few examples of encryption techniques that rely on field theory.
This post is meant to be an introduction to the basics of field theory. With the
introduction out of the way, lets get started!

You can think of a field as a generalized version of the real and rational numbers.
These number sets have some common properties, and we can come up with a more general notion
of what it means for a set to be "like the real numbers". Of course, this is math,
and so we require a more formal definition of a field!

## Definition and Examples

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

$$ x \times y $$ is often denoted as $$ x \cdot y $$, or $$ xy $$.

If you haven't seen these symbols before, this can be kind of intimidating.
Luckily, these symbols have really nice english words associated with them!

 - $$ x\in S $$ is read as "$$ x $$ is an **element of** $$ S $$," where $$ S $$ is a set.
 - $$ \forall $$ is read as "**for all**"
 - $$ \exists $$ is read as "**there exists**"

Reading the above statements in plain english probably makes a bit more sense, but regardless,
I'm sure a list of a bunch of abstract properties is still a bit confusing.
So, I'll give some examples.

As mentioned above, an example of a field is the real numbers, $$ \mathbb{R} $$. You have probably seen properties
**1**, **2**, **5**, **6**, and **7** before. The additive and multiplicative identity elements are $$ 0 $$ and $$ 1 $$
respectively, and the additive inverse of a real number $$ x $$ is $$ -x $$.

For property **9**, the multiplicative inverse of $$ x $$ is $$ x^{-1} $$, which is equal to
$$ \frac{1}{x} $$. So, $$ x \times x^{-1} = x \times \frac{1}{x} = x \div x $$ ! This is
obviously equal to $$ 1 $$!

An example of a set of numbers that isn't a field is the integers, $$ \mathbb{Z} $$.
The integers satisfy properties **1** to **8**, but they don't satisfy property **9**! the multiplicative
inverse of an integer $$ x $$ is a rational number, $$ \frac{1}{x} $$, which is not in the integers for
most values of $$ x $$ (but it is in the rationals, which do form a field!).

## Applications

Well, this is great, but it's not entirely obvious why this is useful. Well, as mentioned at the beginning of this post,
field theory is often useful for developing methods of encryption. In the case of encryption,
we want to be able to decrypt a message that we've encrypted. That is, we want to be able to
_invert_ the encryption process. Well...if you recall properties **4** and **9** from above,
operations in a field are invertible!

Of course, the examples I gave are examples of fields
with an non-finite number of elements in the set, which can cause problems for computers
(since computers have a finite amount of space to work with). Of course, we can develop fields with
a finite number of elements, but that's a discussion for another time!
