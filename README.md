# Doublelength floating point arithmetic

[![Build Status](https://travis-ci.org/jiahao/DoublelengthFloat.jl.svg?branch=master)](https://travis-ci.org/jiahao/DoublelengthFloat.jl)
[![DoublelengthFloat](http://pkg.julialang.org/badges/DoublelengthFloat_release.svg)](http://pkg.julialang.org/?pkg=DoublelengthFloat&ver=release)
[![DoublelengthFloat](http://pkg.julialang.org/badges/DoublelengthFloat_nightly.svg)](http://pkg.julialang.org/?pkg=DoublelengthFloat&ver=nightly)
[![Coverage Status](https://img.shields.io/coveralls/JuliaLang/DoublelengthFloat.jl.svg)](https://coveralls.io/r/jiahao/DoublelengthFloat.jl)

This package provides an implementation of doublelength floating point arithmetic as described in the Reference. The main idea behind doublelength arithmetic is to represent the answer to an operation `a ∘ b` by the sum of two floating-point numbers `z + zz`. The numbers `z` and `zz` are known as the _head_ and _tail_ respectively of the doublelength floating point number; the head is the result of `a ∘ b` in the original precision of the ordinary floating point arithmetic, and the tail is the correction term such that the sum `z + zz`, evaluates as closely as possible to the operation `a ∘ b` in _twice_ the original precision. (The Reference below provides tighter bounds on the precision, but works out to approximately twice the original precision, less 4 bits.)

## The `Doublelength` type

This package defines a new type, `Doublelength`, which wraps the `head` and `tail` components. `Doublelength` is a subtype of `FloatingPoint` and takes a single type parameter `T` which must be a subtype of `FloatingPoint`.

The following methods are defined on `DoublelengthFloat`s:

 Julia method | Name in (Dekker, 1971)
 ------------ | ----------------------
 `+(a::Doublelength{T}, b::Doublelength{T}) -> Doublelength{T}` | `sum2`
 `-(a::Doublelength{T}, b::Doublelength{T}) -> Doublelength{T}` | `sub2`
 `⋅(a::T, b::T) -> Doublelength{T}` | `mul12`
 `*(a::Doublelength{T}, b::Doublelength{T}) -> Doublelength{T}` | `mul2`
 `/(a::Doublelength{T}, b::Doublelength{T}) -> Doublelength{T}` | `div2`
 `√(a::Doublelength{T}) -> Doublelength{T}` (`sqrt`) | `sqrt2`

The elementary operations `+`, `-`, `*` and `/` (together with `sqrt`) are closed under the field of `DoublelengthFloat`s. Additionally, a multiplication method `⋅` is provided which produces a `Doublelength{T}` from multiplying two `T`s together.

## Reference

T. J. Dekker, "A floating-point technique for extending the available precision", _Numerische Mathematik_ 18 (3), **1971**, 224-242. [doi:10.1007/BF01397083](http://link.springer.com/article/10.1007%2FBF01397083)
