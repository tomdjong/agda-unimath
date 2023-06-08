# Joins of left ideals of rings

```agda
module ring-theory.joins-left-ideals-rings where
```

<details><summary>Imports</summary>

```agda
open import foundation.dependent-pair-types
open import foundation.existential-quantification
open import foundation.identity-types
open import foundation.powersets
open import foundation.propositional-truncations
open import foundation.unions-subtypes
open import foundation.universe-levels

open import order-theory.galois-connections-large-posets
open import order-theory.large-suplattices
open import order-theory.least-upper-bounds-large-posets
open import order-theory.similarity-of-elements-large-posets

open import ring-theory.left-ideals-generated-by-subsets-rings
open import ring-theory.left-ideals-rings
open import ring-theory.poset-of-left-ideals-rings
open import ring-theory.products-left-ideals-rings
open import ring-theory.rings
open import ring-theory.subsets-rings
```

</details>

## Idea

The **join** of a family of [left ideals](ring-theory.left-ideals-rings.md) of
[rings](ring-theory.rings.md) is the least left ideal containing all the left
ideals in the family of left ideals. In other words, the join of a family of
left ideals is the
[left ideal generated by](ring-theory.left-ideals-generated-by-subsets-rings.md)
the union of the underlying subsets of the left ideals in the family of left
ideals.

## Definitions

### The predicate of being the join of a family of left ideals

```agda
module _
  {l1 l2 l3 : Level} (R : Ring l1) {U : UU l2} (I : U → left-ideal-Ring l3 R)
  where

  is-join-family-of-left-ideals-Ring :
    {l4 : Level} → left-ideal-Ring l4 R → UUω
  is-join-family-of-left-ideals-Ring =
    is-least-upper-bound-family-of-elements-Large-Poset
      ( left-ideal-Ring-Large-Poset R)
      ( I)

  inclusion-is-join-family-of-left-ideals-Ring :
    {l4 l5 : Level} (J : left-ideal-Ring l4 R) →
    is-join-family-of-left-ideals-Ring J →
    (K : left-ideal-Ring l5 R) → ((α : U) → leq-left-ideal-Ring R (I α) K) →
    leq-left-ideal-Ring R J K
  inclusion-is-join-family-of-left-ideals-Ring J H K =
    pr1 (H K)

  contains-left-ideal-is-join-family-of-left-ideals-Ring :
    {l4 : Level} (J : left-ideal-Ring l4 R) →
    is-join-family-of-left-ideals-Ring J →
    {α : U} → leq-left-ideal-Ring R (I α) J
  contains-left-ideal-is-join-family-of-left-ideals-Ring J H {α} =
    pr2 (H J) (refl-leq-left-ideal-Ring R J) α
```

### The join of a family of left ideals

```agda
module _
  {l1 l2 l3 : Level} (R : Ring l1) {U : UU l2} (I : U → left-ideal-Ring l3 R)
  where

  generating-subset-join-family-of-left-ideals-Ring :
    subset-Ring (l2 ⊔ l3) R
  generating-subset-join-family-of-left-ideals-Ring =
    union-family-of-subtypes (λ α → subset-left-ideal-Ring R (I α))

  join-family-of-left-ideals-Ring :
    left-ideal-Ring (l1 ⊔ l2 ⊔ l3) R
  join-family-of-left-ideals-Ring =
    left-ideal-family-of-subsets-Ring R (λ α → subset-left-ideal-Ring R (I α))

  forward-inclusion-is-join-join-family-of-left-ideals-Ring :
    {l4 : Level} (K : left-ideal-Ring l4 R) →
    ((α : U) → leq-left-ideal-Ring R (I α) K) →
    leq-left-ideal-Ring R join-family-of-left-ideals-Ring K
  forward-inclusion-is-join-join-family-of-left-ideals-Ring K H =
    is-left-ideal-generated-by-family-of-subsets-left-ideal-family-of-subsets-Ring
      ( R)
      ( λ α → subset-left-ideal-Ring R (I α))
      ( K)
      ( λ α x → H α x)

  backward-inclusion-is-join-join-family-of-left-ideals-Ring :
    {l4 : Level} (K : left-ideal-Ring l4 R) →
    leq-left-ideal-Ring R join-family-of-left-ideals-Ring K →
    (α : U) → leq-left-ideal-Ring R (I α) K
  backward-inclusion-is-join-join-family-of-left-ideals-Ring K H _ x p =
    H ( x)
      ( contains-subset-left-ideal-family-of-subsets-Ring R
        ( λ α → subset-left-ideal-Ring R (I α))
        ( x)
        ( p))

  is-join-join-family-of-left-ideals-Ring :
    is-join-family-of-left-ideals-Ring R I join-family-of-left-ideals-Ring
  pr1 (is-join-join-family-of-left-ideals-Ring K) =
    forward-inclusion-is-join-join-family-of-left-ideals-Ring K
  pr2 (is-join-join-family-of-left-ideals-Ring K) =
    backward-inclusion-is-join-join-family-of-left-ideals-Ring K

  inclusion-join-family-of-left-ideals-Ring :
    {l4 : Level} (J : left-ideal-Ring l4 R) →
    ((α : U) → leq-left-ideal-Ring R (I α) J) →
    leq-left-ideal-Ring R join-family-of-left-ideals-Ring J
  inclusion-join-family-of-left-ideals-Ring =
    inclusion-is-join-family-of-left-ideals-Ring R I
      ( join-family-of-left-ideals-Ring)
      ( is-join-join-family-of-left-ideals-Ring)

  contains-left-ideal-join-family-of-left-ideals-Ring :
    {α : U} → leq-left-ideal-Ring R (I α) join-family-of-left-ideals-Ring
  contains-left-ideal-join-family-of-left-ideals-Ring =
    contains-left-ideal-is-join-family-of-left-ideals-Ring R I
      ( join-family-of-left-ideals-Ring)
      ( is-join-join-family-of-left-ideals-Ring)
```

### The large suplattice of left ideals in a ring

```agda
module _
  {l1 : Level} (R : Ring l1)
  where

  is-large-suplattice-left-ideal-Ring-Large-Poset :
    is-large-suplattice-Large-Poset l1 (left-ideal-Ring-Large-Poset R)
  sup-has-least-upper-bound-family-of-elements-Large-Poset
    ( is-large-suplattice-left-ideal-Ring-Large-Poset I) =
    join-family-of-left-ideals-Ring R I
  is-least-upper-bound-sup-has-least-upper-bound-family-of-elements-Large-Poset
    ( is-large-suplattice-left-ideal-Ring-Large-Poset I) =
    is-join-join-family-of-left-ideals-Ring R I

  left-ideal-Ring-Large-Suplattice :
    Large-Suplattice (λ l2 → l1 ⊔ lsuc l2) (λ l2 l3 → l1 ⊔ l2 ⊔ l3) l1
  large-poset-Large-Suplattice
    left-ideal-Ring-Large-Suplattice =
    left-ideal-Ring-Large-Poset R
  is-large-suplattice-Large-Suplattice
    left-ideal-Ring-Large-Suplattice =
    is-large-suplattice-left-ideal-Ring-Large-Poset
```

## Properties

### If `I α ⊆ J α` for each `α`, then `⋁ I ⊆ ⋁ J`

```agda
module _
  {l1 l2 l3 l4 : Level} (A : Ring l1)
  {U : UU l2}
  (I : U → left-ideal-Ring l3 A)
  (J : U → left-ideal-Ring l4 A)
  (H : (α : U) → leq-left-ideal-Ring A (I α) (J α))
  where

  preserves-order-join-family-of-left-ideals-Ring :
    leq-left-ideal-Ring A
      ( join-family-of-left-ideals-Ring A I)
      ( join-family-of-left-ideals-Ring A J)
  preserves-order-join-family-of-left-ideals-Ring =
    preserves-order-sup-Large-Suplattice
      ( left-ideal-Ring-Large-Suplattice A)
      { x = I}
      { y = J}
      ( H)
```

### The operation `S ↦ (S)` preserves joins

```agda
module _
  {l1 l2 l3 : Level} (R : Ring l1) {U : UU l2} (S : U → subset-Ring l3 R)
  where

  is-least-upper-bound-join-left-ideal-subset-Ring :
    is-least-upper-bound-family-of-elements-Large-Poset
      ( left-ideal-Ring-Large-Poset R)
      ( λ α → left-ideal-subset-Ring R (S α))
      ( left-ideal-subset-Ring R (union-family-of-subtypes S))
  is-least-upper-bound-join-left-ideal-subset-Ring =
    preserves-least-upper-bounds-left-ideal-subset-Ring R S
      ( union-family-of-subtypes S)
      ( is-least-upper-bound-union-family-of-subtypes S)

  sim-preserves-join-left-ideal-subset-Ring :
    sim-left-ideal-Ring R
      ( left-ideal-subset-Ring R (union-family-of-subtypes S))
      ( join-family-of-left-ideals-Ring R
        ( λ α → left-ideal-subset-Ring R (S α)))
  sim-preserves-join-left-ideal-subset-Ring =
    sim-is-least-upper-bound-family-of-elements-Large-Poset
      ( left-ideal-Ring-Large-Poset R)
      { x = λ α → left-ideal-subset-Ring R (S α)}
      { y = left-ideal-subset-Ring R (union-family-of-subtypes S)}
      { z =
        join-family-of-left-ideals-Ring R
          ( λ α → left-ideal-subset-Ring R (S α))}
      ( is-least-upper-bound-join-left-ideal-subset-Ring)
      ( is-join-join-family-of-left-ideals-Ring R
        ( λ α → left-ideal-subset-Ring R (S α)))

  preserves-join-left-ideal-subset-Ring :
    left-ideal-subset-Ring R (union-family-of-subtypes S) ＝
    join-family-of-left-ideals-Ring R (λ α → left-ideal-subset-Ring R (S α))
  preserves-join-left-ideal-subset-Ring =
    eq-sim-Large-Poset
      ( left-ideal-Ring-Large-Poset R)
      ( left-ideal-subset-Ring R (union-family-of-subtypes S))
      ( join-family-of-left-ideals-Ring R
        ( λ α → left-ideal-subset-Ring R (S α)))
      ( sim-preserves-join-left-ideal-subset-Ring)
```