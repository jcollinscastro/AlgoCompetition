# Algorithm Competition

A particular operation on eight integers which returns a new set of eight
integers is defined below. The goal is to write an algorithm that implements
this operation with as small a computational cost as possible.

A python utility is supplied which allows one to write their algorithm in
python and easily check its correctness as well as have its computational cost
calculated with some chosen inputs.


# The math puzzle

This problem can be approached with basic algebra skills supplemented with
knowledge of how to solve linear equations involving integers.

What follows is an algebra description of the problem, which skips the deeper
math. For those wanting to explore the details themselves, there is a reference section at the end with links to some helpful material.

```
Consider a cube with integers on each corner,
and the values labelled a,b,c,d,e,f,g,h.

       e ---- f
      /|     /|
     / |    / |
    a ---- b  |
    |  g --|- h
    | /    | /
    |/     |/
    c ---- d


Associated with these 8 cube values are three tuples of values
(A1,B1,C1), (A2,B2,C2), (A3,B3,C3). Given by what will be referred to as the
"cube equations":

    A1 = bc - ad
    A2 = ce - ag
    A3 = be - af
    B1 = -ah + bg + cf - de
    B2 = -ah - bg + cf + de
    B3 = -ah + bg - cf + de
    C1 = fg - eh
    C2 = df - bh
    C3 = dg - ch
```

Given any cube values a,b,c,d,e,f,g,h, it is clear that the tuple values A1,A2,etc. are uniquely determined.

What is not obvious from this, is that given any tuple values (A1,B1,C1), (A2,B2,C2), (A3,B3,C3), if there is a solution, it is unique up to an overall sign on the values a,b,c,d,e,f,g,h.

So up to an overall sign, a cube can be uniquely referred to by either the
eight corner values or the three tuples.

## Function to create

In the competition, your function will be given (a,b,c,d,e,f,g,h)
specifying a cube, with the guarantee that the first two tuples are equal
(A1,B1,C1) = (A2,B2,C2).

The goal is to calculate a new cube such that
```
   (A1',B1',C1') = (A2',B2',C2') = (A3,B3,C3)
```
with as little computational cost as possible. This function will be tested
by giving it an initial value and then repeated application of the function on
its own output. So it is also advantageous to keep the size of the integers in
your cube solution from getting larger each application.

(As noted in the 'Further math information', technically the last equal sign
may be relaxed to an equivalence (A2',B2',C2') ~ (A3,B3,C3), allowing more
freedom in the solution if desired. Pursuing this is not strictly necessary.)


# Useful algebra

Starting with the original 9 equations, for some purposes it is convenient to
expand them into the following 12 equations.
```
  bc - ad = A1
  ce - ag = A2
  be - af = A3
  cf - ah = (B1 + B2)/2
  bg - de = (B1 - B2)/2
  bg - ah = (B1 + B3)/2
  cf - de = (B1 - B3)/2
  de - ah = (B2 + B3)/2
  cf - bg = (B2 - B3)/2
  fg - eh = C1
  df - bh = C2
  dg - ch = C3
```

These can then be manipulated to form more linear looking relationships that must hold for any cube solution.
```
  a C3 + g A1 = c (B1 + B3)/2
  a C1 + g A3 = e (B1 + B3)/2
  b C3 + h A1 = d (B1 + B3)/2
  b C1 + h A3 = f (B1 + B3)/2

  e A1 - c A3 = a (B1 - B3)/2
  f A1 - d A3 = b (B1 - B3)/2
  c C1 - e C3 = g (B1 - B3)/2
  d C1 - f C3 = h (B1 - B3)/2

  e A1 - b A2 = a (B1 - B2)/2
  g A1 - d A2 = c (B1 - B2)/2
  b C1 - e C2 = f (B1 - B2)/2
  d C1 - g C2 = h (B1 - B2)/2

  a C2 + f A1 = b (B1 + B2)/2
  c C2 + h A1 = d (B1 + B2)/2
  a C1 + f A2 = e (B1 + B2)/2
  c C1 + h A2 = g (B1 + B2)/2

  b A2 - c A3 = a (B2 - B3)/2
  f A2 - g A3 = e (B2 - B3)/2
  c C2 - b C3 = d (B2 - B3)/2
  g C2 - f C3 = h (B2 - B3)/2

  d A3 + a C2 = b (B3 + B2)/2
  a C3 + d A2 = c (B3 + B2)/2
  h A3 + e C2 = f (B3 + B2)/2
  e C3 + h A2 = g (B3 + B2)/2
```

If given all the tuple values, this is now a system of linear equations for the
cube values. Of the 24 linear equations, only 6 are linearly independent, so
the 8 cube values can be solved with 2 freedoms remaining.

These freedoms are just from the linear equations not specifying all of the
original constraints. For example it is clear setting all the cube values to
zero would satisfy the linear equations, but not the original equations.

So choosing some non-zero tuple value, the original quadratic equation
can be used to constraint the final 2 freedoms (as a quadratic form equal to
a constant). Therefore, giving a unique solution up to an overall sign.


# Deeper math details

## Composition

It can be proven that if given (A2,B2,C2) and (A3,B3,C3) such that
* the values are relatively prime gcd(A2,B2,C2) = gcd(A3,B3,C3) = 1
* and B2^2 - 4 A2 C2 = B3^2 - 4 A3 C3
then necessarily
* there exists a solution to the cube equations
* the solution is not unique, but are related in a simple way that will be explored shortly.

This can be use to define the following property: (A1,-B1,C1) is said to be a
"composition" of the tuples (A2,B2,C2), (A3,B3,C3) if such a cube exists.

Due to symmetry of the cube and the equations, this can also be said for the
other tuples. Given a solution, we can also say (A2,-B2,C2) is a "composition"
of the tuples (A1,B1,C1), (A3,B3,C3).  And (A3,-B3,C3) is a "composition" of
the tuples (A1,B1,C1), (A2,B2,C2).

In can be checked that for any solution,
```
  B1^2 - 4 A1 C1 = B2^2 - 4 A2 C2 = B3^2 - 4 A3 C3
```
This value is called the discriminant. As mentioned above, this value is
important for the existence of solutions given just two tuples. So in what
follows, let's restrict consideration of tuples to those of some given
discriminant. For convenience, the discriminant will be taken to be -p where
p is a prime number and p+1 is a multiple of 8. This makes it so we know there
exist some tuples of this discriminant, with easily producible examples:
(1,1,(p+1)/4) and (2,1,(p+1)/8).

With this restriction going forward, for any two tuples under consideration
there will always exist a third which is a composition of those two tuples.


## Equivalence

Define two tuples (A,B,C) and (A',B',C') to be "equivalent" if there exists two
other tuples T1 and T2 such that (A,B,C) is a composition of T1
and T1, and (A',B',C') is also a composition of T1 and T2. We will
denote that two tuples are equivalent by writing tuple1 ~ tuple2.


## Composition respects equivalence

The composition property respects the equivalence relation in the following
way. If T1 ~ T2 and T3 ~ T4, then any tuple which is a
composition of T1 and T3 is equivalent to any tuple which is a
composition of T2 and T4.

We can rewrite this more cleanly if we define "*" between tuples to mean
composition, so that (tuple1 * tuple2) as an operation results in some tuple
such that is the composition of tuple1 and tuple2. The previous result can
then be written:
```
    if  T1 ~ T2  and  T3 ~ T4,  then  (T1 * T3) ~ (T2 * T4)
```

## Further properties of composition

It turns out that this operation has nice properties.

* commutative: (T1 * T2) = (T2 * T1)
* associative: (T1 * (T2 * T3)) = ((T1 * T2) * T3)

Reminding that we are restricting to considering the set of tuples with a
particular discriminant, we can further say

* closed: for any two tuple T1,T2 in this set, there exists a T3 which is a composition of T1 * T2.
* identity: there exists a tuple T_identity in this set such that for all
forms T2, (T_identity * T2) ~ T2.
* inverse: for every tuple T1 in this set, there exists a tuple T2 such that
(T1 * T2) ~ T_identity.


## composition of cubes

Now consider two cubes given by the tuples T1a,T1b,T1c and T2a,T2b,T2c
respectively. Then we have:
```
cube1: T1a ~ T1b * T1c
cube2: T2a ~ T2b * T2c

then there exist tuples given by
 T3a ~ T1a * T2a
 T3b ~ T1b * T2b
 T3c ~ T1c * T2c
and therefore
 T3a ~ T3b * T3c

and so composition of tuples, along with existence of a cube for any
three tuples that satisfy a composition relation, means that given two
cubes a third exists which is a "composition" of two other cubes.

It is this cube composition which the algorithm contest involves.


## Function to create, viewed as composition of cubes

In the competition, your function will be given (a,b,c,d,e,f,g,h)
specifying a cube, with the guarantee that the first two tuples are equal
(A1,B1,C1) = (A2,B2,C2).

Using cube composition, a new cube must be calculated such that
* the new cube is the old cube composed with itself
* the new cube also has the first two tuples equal (A1',B1',C1')=(A2',B2',C2')

Note:
```
(A3,B3,C3) ~ (A1,B1,C1) * (A2,B2,C2) ~ (A1,B1,C1) * (A1,B1,C1) ~ (A1',B1',C1')
```


# Properties of solutions to the cube equations

The cube equations have some interesting properties.

## New solution with overall sign flip

Since all the tuple values A1,A2,etc. are a bilinear combination of the 8 cube
values, if we invert all the cube values, this does not change the tuple values.

## Preserving Tuple1,Tuple2 and changing Tuple3

Given a cube, there are some simple transformations we can do to the values
which preserves two of the three tuples, and changes the third in a simple way.

```
swap the values according to
    (a',b',c',d', e',f',g',h') = (c,d,-a,-b, g,h,-e,-f)

A1' = b' c' - a' d' = d (-a) - c (-b) = bc - ad = A1
A2' = c' e' - a' g' = (-a) g - c (-e) = ce - ag = A2
A3' = b' e' - a' f' = d g - c h = C3
and so on...

it is found that
    A1',B1',C1' = A1,B1,C1
    A2',B2',C2' = A2,B2,C2
    A3',B3',C3' = C3,-B3,A3
```

another operation preserving Tuple1,Tuple2 and changing Tuple3 is

```
given any integer n
    (a',b',c',d', e',f',g',h') = (a,b,c+an,d+bn, e,f,g+en,h+fn)

C1' = f' g' - e' h' = f (g+en) - e (h+fn) = fg - eh = C1
C2' = d' f' - b' h' = (d+bn) f - b (h+fn) = df - bh = C2
C3' = d' g' - c' h' = (d+bn)(g+en) - (c+an)(h+fn)
                    = (dg-ch) + n(-ah + bg - cf + de) + n^2(be - af)
                    = C3 + n B3 + n^2 A3
and so on...

it is found that
    A1',B1',C1' = A1,B1,C1
    A2',B2',C2' = A2,B2,C2
    A3',B3',C3' = A3, B3 + 2n A3, C3 + n B3 + n^2 A3
```

The previous two manipulations can be rephrased nicely in the language
of linear algebra

```
first
    |a' b' e' f'| = | 0 1| |a b e f|
    |c' d' g' h'|   |-1 0| |c d g h|

    | A3'  B3'/2| = | 0 1| | A3  B3/2| |0 -1|
    |B3'/2  C3' |   |-1 0| |B3/2  C3 | |1  0|


seond
    |a' b' e' f'| = |1 0| |a b e f|
    |c' d' g' h'|   |n 1| |c d g h|

    | A3'  B3'/2| = |1 0| | A3  B3/2| |1 n|
    |B3'/2  C3' |   |n 1| |B3/2  C3 | |0 1|

```

These two manipulations can combined.


```
general case
modify with any matrix such that ru - st = 1
    |a' b' e' f'| = |r s| |a b e f|
    |c' d' g' h'|   |t u| |c d g h|

    | A3'  B3'/2| = |r s| | A3  B3/2| |r t|
    |B3'/2  C3' |   |t u| |B3/2  C3 | |s u|

```

This freedom in the tuple is precisely the freedom in the
equivalance relation mentioned above.


## Preserving Tuple1,Tuple3 or Tuple2,Tuple3

By symmetry of the cube and equations, similar manipulations can be
done which only change Tuple2 or only change Tuple1.


# Reference

* solving linear integer equations, ax + by = c
  * wikipedia: Extended Euclidean algorithm, [link](https://en.wikipedia.org/wiki/Extended_Euclidean_algorithm)
  * wikipedia: Bezout's identity, [link](https://en.wikipedia.org/wiki/B%C3%A9zout%27s_identity)
  * textbook style discussion of solutions, [link](http://gauss.math.luc.edu/greicius/Math201/Fall2012/Lectures/linear-diophantine.article.pdf)
*  binary quadratic forms
  *  wikipedia: binary quadratic forms, [link](https://en.wikipedia.org/wiki/Binary_quadratic_form)
  *  introduction by Lipa Long from Chia Networks, [link](https://github.com/Chia-Network/vdf-competition/blob/master/classgroups.pdf)
* Bhargava cubes
  *  wikipedia: Bhargava cubes, [link](https://en.wikipedia.org/wiki/Bhargava_cube)
  *  original article (math jargon heavy) by Bhargava, "Higher composition laws I:
A new view on Gauss composition, and quadratic generalizations", [pdf](https://annals.math.princeton.edu/wp-content/uploads/annals-v159-n1-p03.pdf)


# Acknowledgements

inkfish from https://github.com/Chia-Network/vdf-competition.git

