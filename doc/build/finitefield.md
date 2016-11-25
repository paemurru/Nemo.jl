


<a id='Introduction-1'></a>

## Introduction


Finite fields are provided in Nemo by Flint. This allows construction of finite fields of any characteristic and degree for which there are Conway polynomials. It is also possible for the user to specify their own irreducible polynomial generating a finite field.


Finite fields are constructed using the `FlintFiniteField` function. However, for convenience we define


```
FiniteField = FlintFiniteField
```


so that finite fields can be constructed using `FiniteField` rather than `FlintFiniteField`. Note that this is the name of the constructor, but not of finite field type.


The types of finite field elements in Nemo are given in the following table, along with the libraries that provide them and the associated types of the parent objects.


| Library |                          Field | Element type |         Parent type |
| -------:| ------------------------------:| ------------:| -------------------:|
|   Flint | $\mathbb{F}_{p^n}$ (small $p$) |    `fq_nmod` | `FqNmodFiniteField` |
|   Flint | $\mathbb{F}_{p^n}$ (large $p$) |         `fq` |     `FqFiniteField` |


The only difference between the `fq` and `fq_nmod` types is the representation. The former is for finite fields with multiprecision characteristic and the latter is for characteristics that fit into a single unsigned machine word. The `FlintFiniteField` constructor automatically picks the correct representation for the user, and so the average user doesn't need to know about the actual types.


All the finite field types belong to the `FinField` abstract type and the finite field element types belong to the `FinFieldElem` abstract type.


Since all the functionality for the `fq` finite field type is identical to that provided for the `fq_nmod` finite field type, we simply document the former.


<a id='Finite-field-constructors-1'></a>

## Finite field constructors


In order to construct finite field elements in Nemo, one must first construct the finite field itself. This is accomplished with one of the following constructors.

<a id='Nemo.FlintFiniteField-Tuple{Nemo.fmpz,Int64,AbstractString}' href='#Nemo.FlintFiniteField-Tuple{Nemo.fmpz,Int64,AbstractString}'>#</a>
**`Nemo.FlintFiniteField`** &mdash; *Method*.



```
FlintFiniteField(char::fmpz, deg::Int, s::AbstractString{})
```

> Returns a tuple $S, x$ consisting of a finite field parent object $S$ and generator $x$ for the finite field of the given characteristic and degree. The string $s$ is used to designate how the finite field generator will be printed. The characteristic must be prime. When a Conway polynomial is known, the field is generated using the Conway polynomial. Otherwise a random sparse, irreducible polynomial is used. The generator of the field is  guaranteed to be a multiplicative generator only if the field is generated by a Conway polynomial. We require the degree to be positive.



<a target='_blank' href='https://github.com/wbhart/Nemo.jl/tree/2d9f699d07b271409d36504c459e30f3e8d24ffb/src/flint/fq.jl#L506' class='documenter-source'>source</a><br>

<a id='Nemo.FlintFiniteField-Tuple{Integer,Int64,AbstractString}' href='#Nemo.FlintFiniteField-Tuple{Integer,Int64,AbstractString}'>#</a>
**`Nemo.FlintFiniteField`** &mdash; *Method*.



```
FlintFiniteField(char::Integer, deg::Int, s::AbstractString{})
```

> Returns a tuple $S, x$ consisting of a finite field parent object $S$ and generator $x$ for the finite field of the given characteristic and degree. The string $s$ is used to designate how the finite field generator will be printed. The characteristic must be prime. When a Conway polynomial is known, the field is generated using the Conway polynomial. Otherwise a random sparse, irreducible polynomial is used. The generator of the field is  guaranteed to be a multiplicative generator only if the field is generated by a Conway polynomial. We require the degree to be positive.



<a target='_blank' href='https://github.com/wbhart/Nemo.jl/tree/2d9f699d07b271409d36504c459e30f3e8d24ffb/src/flint/fq.jl#L524' class='documenter-source'>source</a><br>

<a id='Nemo.FlintFiniteField-Tuple{Nemo.fmpz_mod_poly,AbstractString}' href='#Nemo.FlintFiniteField-Tuple{Nemo.fmpz_mod_poly,AbstractString}'>#</a>
**`Nemo.FlintFiniteField`** &mdash; *Method*.



```
FlintFiniteField(pol::fmpz_mod_poly, s::AbstractString{})
```

> Returns a tuple $S, x$ consisting of a finite field parent object $S$ and generator $x$ for the finite field over $F_p$ defined by the given polynomial, i.e. $\mathbb{F}_p[t]/(pol)$. The characteristic is specified by the modulus of `pol`. The polynomial is required to be irreducible, but this is not checked. The string $s$ is used to designate how the finite field generator will be printed. The generator will not be multiplicative in general.



<a target='_blank' href='https://github.com/wbhart/Nemo.jl/tree/2d9f699d07b271409d36504c459e30f3e8d24ffb/src/flint/fq.jl#L539' class='documenter-source'>source</a><br>


Here are some examples of creating finite fields and making use of the resulting parent objects to coerce various elements into those fields.


```
R, x = FiniteField(7, 3, "x")
S, y = FiniteField(ZZ(12431351431561), 2, "y")
T, t = PolynomialRing(ResidueRing(ZZ, 12431351431561), "t")
U, z = FiniteField(t^2 + 7, "z")

a = R(5)
b = R(x)
c = S(ZZ(11))
d = U(7)
```


<a id='Finite-field-element-constructors-1'></a>

## Finite field element constructors


Once a finite field is constructed, there are various ways to construct elements in that field.


Apart from coercing elements into the finite field as above, we offer the following functions.

<a id='Base.zero-Tuple{Nemo.FqFiniteField}' href='#Base.zero-Tuple{Nemo.FqFiniteField}'>#</a>
**`Base.zero`** &mdash; *Method*.



```
zero(a::FqFiniteField)
```

> Return the additive identity, zero, in the given finite field.



<a target='_blank' href='https://github.com/wbhart/Nemo.jl/tree/2d9f699d07b271409d36504c459e30f3e8d24ffb/src/flint/fq.jl#L70' class='documenter-source'>source</a><br>

<a id='Base.one-Tuple{Nemo.FqFiniteField}' href='#Base.one-Tuple{Nemo.FqFiniteField}'>#</a>
**`Base.one`** &mdash; *Method*.



```
one(a::FqFiniteField)
```

> Return the multiplicative identity, one, in the given finite field.



<a target='_blank' href='https://github.com/wbhart/Nemo.jl/tree/2d9f699d07b271409d36504c459e30f3e8d24ffb/src/flint/fq.jl#L80' class='documenter-source'>source</a><br>

<a id='Nemo.gen-Tuple{Nemo.FqFiniteField}' href='#Nemo.gen-Tuple{Nemo.FqFiniteField}'>#</a>
**`Nemo.gen`** &mdash; *Method*.



```
gen(a::FqFiniteField)
```

> Return the generator of the finite field. Note that this is only guaranteed to be a multiplicative generator if the finite field is generated by a Conway polynomial automatically.



<a target='_blank' href='https://github.com/wbhart/Nemo.jl/tree/2d9f699d07b271409d36504c459e30f3e8d24ffb/src/flint/fq.jl#L90' class='documenter-source'>source</a><br>


Here are some examples of constructing finite field elements.


```
R, x = FiniteField(ZZ(7), 5, "x")

a = zero(R)
b = one(R)
c = gen(R)
```


<a id='Basic-functionality-1'></a>

## Basic functionality


The following basic functionality is provided by the default finite field implementation in Nemo, to support construction of generic rings over finite fields. Any custom finite field implementation in Nemo should provide these  functions along with the usual arithmetic operations.


```
parent_type(::Type{fq})
```


Gives the type of the parent object of a Flint finite field element.


```
elem_type(R::FqFiniteField)
```


Given the parent object for a finite field, return the type of elements of the field.


```
Base.hash(a::fq, h::UInt)
```


Return a `UInt` hexadecimal hash of the finite field element $a$. This should be xor'd with a fixed random hexadecimal specific to the finite field type. The hash of the coefficients of the finite field representation should be xor'd with the supplied parameter `h` as part of computing the hash.


```
deepcopy(a::fq)
```


Construct a copy of the given finite field element and return it. This function must recursively construct copies of all of the internal data in the given element. Nemo finite field elements are mutable and so returning shallow copies is not sufficient.


```
mul!(c::fq, a::fq, b::fq)
```


Multiply $a$ by $b$ and set the existing finite field element $c$ to the result. This function is provided for performance reasons as it saves allocating a new object for the result and eliminates associated garbage collection.


```
addeq!(c::fq, a::fq)
```


In-place addition. Adds $a$ to $c$ and sets $c$ to the result. This function is provided for performance reasons as it saves allocating a new object for the result and eliminates associated garbage collection.


Given the parent object `R` for a finite field, the following coercion functions are provided to coerce various elements into the finite field. Developers provide these by overloading the `call` operator for the finite field parent objects.


```
R()
```


Coerce zero into the finite field.


```
R(n::Integer)
R(f::fmpz)
```


Coerce an integer value into the finite field.


```
R(f::fq)
```


Take a finite field element that is already in the finite field and simply return it. A copy of the original is not made.


In addition to the above, developers of custom finite field types must ensure that they provide the equivalent of the function `base_ring(R::FqFiniteField)` which should return `Union{}`. In addition to this they should ensure that each finite field element contains a field `parent` specifying the parent object of the finite field element, or at least supply the equivalent of the function `parent(a::fq)` to return the parent object of a finite field element.


<a id='Basic-manipulation-1'></a>

## Basic manipulation


Numerous functions are provided to manipulate finite field elements. Also see the section on basic functionality above.

<a id='Nemo.base_ring-Tuple{Nemo.FqFiniteField}' href='#Nemo.base_ring-Tuple{Nemo.FqFiniteField}'>#</a>
**`Nemo.base_ring`** &mdash; *Method*.



```
base_ring(a::FqFiniteField)
```

> Returns `Union{}` as this field is not dependent on another field.



<a target='_blank' href='https://github.com/wbhart/Nemo.jl/tree/2d9f699d07b271409d36504c459e30f3e8d24ffb/src/flint/fq.jl#L20' class='documenter-source'>source</a><br>

<a id='Nemo.base_ring-Tuple{Nemo.fq}' href='#Nemo.base_ring-Tuple{Nemo.fq}'>#</a>
**`Nemo.base_ring`** &mdash; *Method*.



```
base_ring(a::fq)
```

> Returns `Union{}` as this field is not dependent on another field.



<a target='_blank' href='https://github.com/wbhart/Nemo.jl/tree/2d9f699d07b271409d36504c459e30f3e8d24ffb/src/flint/fq.jl#L26' class='documenter-source'>source</a><br>

<a id='Base.parent-Tuple{Nemo.fq}' href='#Base.parent-Tuple{Nemo.fq}'>#</a>
**`Base.parent`** &mdash; *Method*.



```
parent(a::fq)
```

> Returns the parent of the given finite field element.



<a target='_blank' href='https://github.com/wbhart/Nemo.jl/tree/2d9f699d07b271409d36504c459e30f3e8d24ffb/src/flint/fq.jl#L32' class='documenter-source'>source</a><br>

<a id='Nemo.iszero-Tuple{Nemo.fq}' href='#Nemo.iszero-Tuple{Nemo.fq}'>#</a>
**`Nemo.iszero`** &mdash; *Method*.



```
iszero(a::fq)
```

> Return `true` if the given finite field element is zero, otherwise return `false`.



<a target='_blank' href='https://github.com/wbhart/Nemo.jl/tree/2d9f699d07b271409d36504c459e30f3e8d24ffb/src/flint/fq.jl#L102' class='documenter-source'>source</a><br>

<a id='Nemo.isone-Tuple{Nemo.fq}' href='#Nemo.isone-Tuple{Nemo.fq}'>#</a>
**`Nemo.isone`** &mdash; *Method*.



```
isone(a::fq)
```

> Return `true` if the given finite field element is one, otherwise return `false`.



<a target='_blank' href='https://github.com/wbhart/Nemo.jl/tree/2d9f699d07b271409d36504c459e30f3e8d24ffb/src/flint/fq.jl#L110' class='documenter-source'>source</a><br>

<a id='Nemo.isunit-Tuple{Nemo.fq}' href='#Nemo.isunit-Tuple{Nemo.fq}'>#</a>
**`Nemo.isunit`** &mdash; *Method*.



```
isunit(a::fq)
```

> Return `true` if the given finite field element is invertible, i.e. nonzero, otherwise return `false`.



<a target='_blank' href='https://github.com/wbhart/Nemo.jl/tree/2d9f699d07b271409d36504c459e30f3e8d24ffb/src/flint/fq.jl#L125' class='documenter-source'>source</a><br>

<a id='Nemo.isgen-Tuple{Nemo.fq}' href='#Nemo.isgen-Tuple{Nemo.fq}'>#</a>
**`Nemo.isgen`** &mdash; *Method*.



```
isgen(a::fq)
```

> Return `true` if the given finite field element is the generator of the finite field, otherwise return `false`.



<a target='_blank' href='https://github.com/wbhart/Nemo.jl/tree/2d9f699d07b271409d36504c459e30f3e8d24ffb/src/flint/fq.jl#L118' class='documenter-source'>source</a><br>

<a id='Nemo.coeff-Tuple{Nemo.fq,Int64}' href='#Nemo.coeff-Tuple{Nemo.fq,Int64}'>#</a>
**`Nemo.coeff`** &mdash; *Method*.



```
coeff(x::fq, n::Int)
```

> Return the degree $n$ coefficient of the polynomial representing the given finite field element.



<a target='_blank' href='https://github.com/wbhart/Nemo.jl/tree/2d9f699d07b271409d36504c459e30f3e8d24ffb/src/flint/fq.jl#L57' class='documenter-source'>source</a><br>

<a id='Nemo.degree-Tuple{Nemo.FqFiniteField}' href='#Nemo.degree-Tuple{Nemo.FqFiniteField}'>#</a>
**`Nemo.degree`** &mdash; *Method*.



```
degree(a::FqFiniteField)
```

> Return the degree of the given finite field.



<a target='_blank' href='https://github.com/wbhart/Nemo.jl/tree/2d9f699d07b271409d36504c459e30f3e8d24ffb/src/flint/fq.jl#L155' class='documenter-source'>source</a><br>

<a id='Nemo.characteristic-Tuple{Nemo.FqFiniteField}' href='#Nemo.characteristic-Tuple{Nemo.FqFiniteField}'>#</a>
**`Nemo.characteristic`** &mdash; *Method*.



```
characteristic(a::FqFiniteField)
```

> Return the characteristic of the given finite field.



<a target='_blank' href='https://github.com/wbhart/Nemo.jl/tree/2d9f699d07b271409d36504c459e30f3e8d24ffb/src/flint/fq.jl#L133' class='documenter-source'>source</a><br>

<a id='Nemo.order-Tuple{Nemo.FqFiniteField}' href='#Nemo.order-Tuple{Nemo.FqFiniteField}'>#</a>
**`Nemo.order`** &mdash; *Method*.



```
order(a::FqFiniteField)
```

> Return the order, i.e. the number of elements in, the given finite field.



<a target='_blank' href='https://github.com/wbhart/Nemo.jl/tree/2d9f699d07b271409d36504c459e30f3e8d24ffb/src/flint/fq.jl#L144' class='documenter-source'>source</a><br>


Here are some examples of basic manipulation of finite field elements.


```
R, x = FiniteField(ZZ(7), 5, "x")

a = zero(R)
b = one(R)
c = gen(R)

d = characteristic(R)
f = order(R)
g = degree(R)
h = iszero(a)
k = isone(b)
m = isunit(x + 1)
n = isgen(x)
U = parent(x + 1)
V = base_ring(R)
```


<a id='Arithmetic-operations-1'></a>

## Arithmetic operations


Nemo provides all the standard field operations for finite field elements, as follows.


|               Function |      Operation |
| ----------------------:| --------------:|
|               -(a::fq) |    unary minus |
|        +(a::fq, b::fq) |       addition |
|        -(a::fq, b::fq) |    subtraction |
|        *(a::fq, b::fq) | multiplication |
| divexact(a::fq, b::fq) | exact division |


In addition, the following ad hoc field operations are defined.


|                    Function |      Operation |
| ---------------------------:| --------------:|
|        +(a::fq, b::Integer) |       addition |
|        +(a::Integer, b::fq) |       addition |
|           +(a::fq, b::fmpz) |       addition |
|           +(a::fmpz, b::fq) |       addition |
|        -(a::fq, b::Integer) |    subtraction |
|        -(a::Integer, b::fq) |    subtraction |
|           -(a::fq, b::fmpz) |    subtraction |
|           -(a::fmpz, b::fq) |    subtraction |
|        *(a::fq, b::Integer) | multiplication |
|        *(a::Integer, b::fq) | multiplication |
|           *(a::fq, b::fmpz) | multiplication |
|           *(a::fmpz, b::fq) | multiplication |
| divexact(a::fq, b::Integer) | exact division |
|    divexact(a::fq, b::fmpz) | exact division |
| divexact(a::Integer, b::fq) | exact division |
|    divexact(a::fmpz, b::fq) | exact division |
|            ^(a::fq, b::Int) |       powering |
|           ^(a::fq, b::fmpz) |       powering |


Here are some examples of arithmetic operations on finite fields.


```
R, x = FiniteField(ZZ(7), 5, "x")

a = x^4 + 3x^2 + 6x + 1
b = 3x^4 + 2x^2 + x + 1

c = a + b
d = a - b
f = a*b
g = 3a
h = b*ZZ(5)
k = divexact(a, b)
m = divexact(1, b)
n = divexact(a, ZZ(2))
p = a^3
```


<a id='Comparison-1'></a>

## Comparison


Nemo provides the comparison operation `==` for finite field elements. Julia then automatically provides the corresponding `!=` operation. Here are the functions provided.


<a id='Function-1'></a>

## Function


==(a::fq, b::fq)


In addition, the following ad hoc comparisons are provided, Julia again providing the corresponding `!=` operators.


<a id='Function-2'></a>

## Function


==(a::fq, b::Integer) ==(a::fq, b::fmpz) ==(a::Integer, b::fq) ==(a::fmpz, b::fq)


Here are some examples of comparisons.


```
R, x = FiniteField(ZZ(7), 5, "x")

a = x^4 + 3x^2 + 6x + 1
b = 3x^4 + 2x^2 + 2

b != a
a == 3
ZZ(5) == b
```


<a id='Inversion-1'></a>

## Inversion

<a id='Base.inv-Tuple{Nemo.fq}' href='#Base.inv-Tuple{Nemo.fq}'>#</a>
**`Base.inv`** &mdash; *Method*.



```
inv(x::fq)
```

> Return $x^{-1}$.



<a target='_blank' href='https://github.com/wbhart/Nemo.jl/tree/2d9f699d07b271409d36504c459e30f3e8d24ffb/src/flint/fq.jl#L378' class='documenter-source'>source</a><br>


Here are some examples of inversion.


```
R, x = FiniteField(ZZ(7), 5, "x")

a = x^4 + 3x^2 + 6x + 1

b = inv(a)
```


<a id='Special-functions-1'></a>

## Special functions


Various special functions with finite field specific behaviour are defined.

<a id='Base.LinAlg.trace-Tuple{Nemo.fq}' href='#Base.LinAlg.trace-Tuple{Nemo.fq}'>#</a>
**`Base.LinAlg.trace`** &mdash; *Method*.



```
trace(x::fq)
```

> Return the trace of $a$. This is an element of $\F_p$, but the value returned is this value embedded in the original finite field.



<a target='_blank' href='https://github.com/wbhart/Nemo.jl/tree/2d9f699d07b271409d36504c459e30f3e8d24ffb/src/flint/fq.jl#L408' class='documenter-source'>source</a><br>

<a id='Base.LinAlg.norm-Tuple{Nemo.fq}' href='#Base.LinAlg.norm-Tuple{Nemo.fq}'>#</a>
**`Base.LinAlg.norm`** &mdash; *Method*.



```
norm(x::fq)
```

> Return the norm of $a$. This is an element of $\F_p$, but the value returned is this value embedded in the original finite field.



<a target='_blank' href='https://github.com/wbhart/Nemo.jl/tree/2d9f699d07b271409d36504c459e30f3e8d24ffb/src/flint/fq.jl#L420' class='documenter-source'>source</a><br>

<a id='Nemo.frobenius-Tuple{Nemo.fq,Int64}' href='#Nemo.frobenius-Tuple{Nemo.fq,Int64}'>#</a>
**`Nemo.frobenius`** &mdash; *Method*.



```
frobenius(x::fq, n = 1)
```

> Return the iterated Frobenius $\sigma_p^n(a)$ where $\sigma_p$ is the  Frobenius map sending the element $a$ to $a^p$ in the finite field of  characteristic $p$. By default the Frobenius map is applied $n = 1$ times if $n$ is not specified.



<a target='_blank' href='https://github.com/wbhart/Nemo.jl/tree/2d9f699d07b271409d36504c459e30f3e8d24ffb/src/flint/fq.jl#L432' class='documenter-source'>source</a><br>

<a id='Nemo.pth_root-Tuple{Nemo.fq}' href='#Nemo.pth_root-Tuple{Nemo.fq}'>#</a>
**`Nemo.pth_root`** &mdash; *Method*.



```
pth_root(x::fq)
```

> Return the $p$-th root of $a$ in the finite field of characteristic $p$. This is the inverse operation to the Frobenius map $\sigma_p$.



<a target='_blank' href='https://github.com/wbhart/Nemo.jl/tree/2d9f699d07b271409d36504c459e30f3e8d24ffb/src/flint/fq.jl#L396' class='documenter-source'>source</a><br>


Here are some examples of special functions.


```
R, x = FiniteField(ZZ(7), 5, "x")

a = x^4 + 3x^2 + 6x + 1

b = trace(a)
c = norm(a)
d = frobenius(a)
f = frobenius(a, 3)
g = pth_root(a)
```
