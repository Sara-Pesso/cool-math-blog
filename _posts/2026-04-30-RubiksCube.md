---
title: "Rubik’s Cube Solvability using Group Theory"
date: 2026-04-30
---
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    tex2jax: {
      inlineMath: [ ['$','$'], ["\\(","\\)"] ],
      processEscapes: true
    }
  });
</script>


# Introduction

In 1878, puzzle maker Sam Loyd released a 15-square puzzle with a prize
of \$1000 for the first person to solve it. Legend has it that "people
became infatuated with puzzle\" in hopes of winning the money. In this
pursuit, "pilots are said to have wrecked their ships, and engineers
rush theirs trains past their stations\", people would claim to have
solved the puzzle only to mysteriously fail to recollect the exact
sequence of moves that solved the puzzle. Ultimately, no one was ever
able to claim the prize from Loyd for one simple reason: the puzzle was
unsolvable-- and using basic group theory, it is easy to
prove![@textbook]\
\
Using the same group theory ideas and applying them to the Rubik's Cube,
we will explore the Rubik's Cube Group for both the 3x3 and 2x2 Rubik's
Cubes. We will also discuss an algorithm to solve the cube, why it
works, and how to make impossible Rubik's Cubes so you, too, can prank
puzzle-solvers everywhere.

# Group Theory in the Rubik's Cube

## Notation

We will represent each cube using Singmaster notation, which represents
a clockwise (cw), 90 degree turn of each side of the Rubik's cube using
a letter, with respect to a stationary observer [@sfunotation]. These
are known as the 6 Basic Moves and are explained further in Figure
[1](#tab:placeholder){reference-type="ref" reference="tab:placeholder"}.

::: {#tab:placeholder}
  --- ---------------------------------------
   R   90 deg. cw rotation of the RIGHT face
   L   90 deg. cw rotation of the LEFT face
   U    90 deg. cw rotation of the UP face
   D   90 deg. cw rotation of the DOWN face
   F   90 deg. cw rotation of the FRONT face
   B   90 deg. cw rotation of the BACK face
  --- ---------------------------------------

  : Table of the 6 Basic Cube Moves
:::

Note that $R', L', U', D',  F', B'$ represent the inverse of each cube
move, or a 90 degree counter clockwise (ccw) rotation of the face.\
\
Comprising the 3x3 Rubik's Cube, there are 26 individual **cubelets**
(or **cubies**). Of these, only 20 are mutable, while the center
cubelets never change position or reorient when moves are applied. There
are 8 **corner cubelets** and 12 **edge cubelets**. Each cubelet
occupies a different **cubicle** (it's position relative to the user).\
These cubelets are comprised of 54 **facelets** (that is, the colorful,
"sticker\" sides of the cubelets). Again, only 48 are mutable, while the
center facelet on each face of the Rubik's Cube never changes.\
\
The 2x2 Rubik's Cube (known as the Pocket Cube) has 8 cubelets (8 corner
and 0 edge cubelets) and 24 facelets, all of which are mutable.

## The Rubik's Cube Group.

Let's briefly discuss how the set of all legal moves of the Rubik's Cube
(which is equivalent to all legal configurations of the Rubik's Cube)
form a group: $(G, \cdot)$. Let $g_1$ and $g_2$ be two moves within the
set of possible Rubik's Cube moves, $g_1, g_2 \in G$, and let $\cdot$ be
defined as performing $g_1$ followed by $g_2$. In order to be a group,
$(G,\cdot)$ must satisfy the following four criteria:

- **Closure.** Under $\cdot$, since $g_1$ and $g_2$ are legal Rubik's
  Cube moves, clearly $g_1\cdot g_2$ is a legal configuration.
  Therefore, $g_1\cdot g_2 \in G$ and $G$ is closed.

- **Identity elements.** Let $e$ be the empty move, where we do nothing
  to the Rubik's Cube. Then $e\in G$ and $\forall g\in G$,
  $e\cdot g = g\cdot e = g$, and $e$ is the identity element.

- **Associativity.** Clearly, $G$ is associative under $\cdot$. Let
  $g_1, g_2, g_3 \in G$, we can see that
  $(g_1\cdot g_2)\cdot g_3 = g_1 \cdot(g_2\cdot g_3)$, because the 6
  Basic Moves that each $g_i$ consists of are associative. Therefore,
  $(G,\cdot)$ is associative.

- **Inverses.** There must exist an inverse for every Rubik's Cube move.
  Earlier, we defined for each basic move $X$, $X'$ as it's inverse--
  which simply performing a 90 degree ccw rotation on the same face. So,
  $\forall g\in G$, we can write $g= (X_1\cdot X_2 ... \cdot X_n)$ where
  $X_1,X_2,...X_n$ are each one of the 6 Basic Moves. Then, we can
  easily find the inverse
  $g' = (X_1\cdot X_2 ... \cdot X_n)^{-1} = X_n'\cdot X_{n-1}'...X_1'$.
  This is the inverse of $g$, because
  $$X_n'\cdot X_{n-1}'...X_1' \cdot(X_1\cdot X_2 ... \cdot X_n) = X_n'\cdot X_{n-1}'...X_1'\cdot (X_1\cdot X_2 ... \cdot X_n) = e$$
  (for a more formal proof by induction see [@chen]). Essentially, just
  do all Basic Moves that comprise $g\in G$ in reverse, and we have the
  inverse.

Now that We have established $(G,\cdot)$ is a group, we can move on to
more interesting explorations. It is also important to note that the
group of Rubik's Cube moves is the same for both the 3x3 and 2x2 Rubik's
Cube. For $n \text{ x } n$ Rubik's Cubes with $n>3$, we would need to
define more moves for the interior, movable cubes, so we will only be
discussing the 3x3 or 2x2 Cubes here.

## Showing $G$ is Non-Abelian.

We can do this with a counter example. Consider $R\cdot U$ and
$U\cdot R$. Performing each of these moves on the Pocket Cube starting
in the solved state, results in the states in Figure
[\[fig:RU\]](#fig:RU){reference-type="ref" reference="fig:RU"} and
Figure [1](#fig:UR){reference-type="ref" reference="fig:UR"},
respectively. Clearly, $RU \not = UR$. Therefore, $G$ is non-abelian in
the 2x2 case.\
\
This result generalizes to the 3x3 Rubik's Cube as well, since the
Pocket Cube represents the corners of the 3x3 Rubik's Cube. Therefore,
$G$ is also non-abelian for the 3x3 Rubik's Cube.

<figure id="fig:UR">
<div class="minipage">
<p><img src="../../../assets/images/Rubiks Cube/RU.JPG" alt="image" /> <span id="fig:RU"
data-label="fig:RU"></span></p>
</div>
<div class="minipage">
<p><img src="../../../assets/images/Rubiks Cube/UR.JPG" alt="image" /> <span id="fig:UR"
data-label="fig:UR"></span></p>
</div>
</figure>

## Generators.

Because we have shown that $G$ is not abelian, we know that it is **not
cyclic**. That is, $\not \exists g \in G$ such that
$<g> = \{g^n: n\in \mathbb{N}\}$ [@textbook]. But, there are some
generating sets. An obvious generator is the set of our 6 Basic Moves
(plus the identity or empty move) with their inverses:
$<e,L,R,U,D,F,B,L',R',U',D',F',B'>$. Maybe less obvious is that just the
6 Basic Moves themselves constitute a generator of $G$ [@Garron], that
is: $$<L,R,U,D,F,B> = G$$ we can undo each of these using combinations
of the other five moves [@Garron]. Also, observe that
$\forall X\in \{R,L,U,D,F,B\}$, $XXXX = X^4 = e$, so the identity is
always available.\
\
Using the previous observation, it becomes more obvious that any one of
the 6 Basic Moves can be generated from the other 5. That is,
$$G = <L,R,U,D,F>$$ $$G = <L,R,U,D,B>$$ $$G = <L,R,U,F,B>$$
$$G = <L,R,D,F,B>$$ $$G = <L,U,D,F,B>$$ $$G = <R,U,D,F,B>$$ are all
generating sets, and $(G,\cdot)$ is **five-generated** [@Garron]. An
example from Garron's paper is that

$$D = (L' R FF BB L' R) U (L' R FF BB L' R)$$

Furthermore, $G$ is **two-generated** when using compound moves. An
example from Garron is $$G = <UBLUL'U'B', RRFLD'R'>$$

## Subgroups.

Recall that a subgroup, $H$, is a subset of a group that is a group
itself under the same operation [@textbook]. This written as $H \leq G$.
In fact, $G\leq S_{54}, G\leq S_{48}$ for the 3x3 Rubik's Cube
[@Bobzien], and in the Pocket Cube case $G\leq S_{24}$.\
\
Recall that the symmetric group, $S_n$, consists permutations of a set
of $n$ items. In the case of $S_{54}$, we can see that the 3x3 Rubik's
Cube has 54 facelets that can be permuted, but as aforementioned not all
these permutations are possible. Therefore, $G \leq S_{54}$. The case
for $S_{48}$ is similar, except here we observe that that center facelet
on each face of the Rubik's Cube never move, leaving only 48 facelets to
permute. Again, not all permutations of these 48 facelets are legal, so
it follows that $G \leq S_{48}$ [@Bobzien].\
Similarly, the Pocket Cube is $G \leq S_{24}$. It has 24 facelets that
can be permuted, but not all these permutations are legal.\
\
More interesting are the subgroups of $(G, \cdot)$, Garron lays out many
examples, the obvious being [@textbook]:

- $G\leq G$

- $e \leq G$

Unfortunately, because $(G,\cdot)$ is not cyclic, the number and orders
of the subgroups is not obvious. So, we will just discuss an interesting
one here:\
\
For any $g \in G$, $<g> \leq G$ [@Garron]. Garron doesn't prove this in
his paper, so we will show it here:

- **Subset.** $<g> \subset G$, since $g\in G$ and $G$ is closed under
  $\cdot$

- **Closure**. Because $<g> = \{g^n:n\in\mathbb{N}\}$, any
  $g_1,g_2 \in <g>$ take the form $g_1 = g^n$, $g_2 = g^m$. Therefore,
  $g_1\cdot g_2 = g^{n+m} \in <g>$ by definition of a generator, so
  $<g>$ is closed under $\cdot$

- **Identity element.** In order for $<g>$ to be a subgroup of
  $(G,\cdot)$, $e_G \in <g>$. Recall that $e_G$ is the empty move. We
  know that repeating the same move over and over will eventually result
  in original state of the cube [@Garron]. This means $\exists k$ such
  that $g^k = e_G$ $\forall g\in G$. Since $g^k \in <g>$, it follows
  that $e_G\in <g>, \forall g\in G$.

- **Inverses.** Recall that for an element $x \in <g>$ the inverse is
  $x'\in <g>$, such that $xx' = x'x = e_G$. Let $x' = g^{k-1}$, where
  $k$ is such that $g^k = e_G$ from the proof of the Identity Element.
  Then, $xx' = x'x = e_G$ becomes
  $$g\cdot g^{k-1} = g^{k-1}\cdot g = e_G$$ which holds
  $\forall g\in G$. Therefore, the inverses of all elements exist in
  $<g>$.

- **Associativity.** Associativity is inherited from $G$.

Therefore, we have proven that for any legal move $g\in G$,
$<g> \leq G$. Unlike $G$, this subgroup is clearly abelian, since only
one move is involved. This also means that the generators we discussed
in Section 2.4 Generators are also subgroups of $G$.

## Normal Subgroups.

According to the Rubik's Cube Group Wikipedia page [@wiki], the group
$C_o$, or the subset of moves that leaves every cubelet fixed, but
change their orientations is a normal subgroup. They omit the proof of
this, so we will verify it here. First, let's make sure $C_o \leq G$ by
making sure it fulfills the subgroup criteria:

1.  **Subset.** Clearly, $C_o \subset G$, by definition since these are
    legal moves that also have the special property of leaving the
    cubelets fixed, but rotating the facelets.

2.  **Identity element.** Recall $e_G$ is the empty move. Therefore,
    $e_G \in C_o$ since the cubelets are left fixed by $e_G$.

3.  **Closure.** Let $A,B \in C_o$, then $A\cdot B \in C_o$ because
    neither $A$ nor $B$ move any cubelets, and therefore their
    composition $AB$ also will not move any cubelets.

4.  **Inverses.** Recall that $\forall c \in C_o$, $c'$ is the move that
    undoes $c$, such that $cc' = c'c = e$. If we have configuration
    $g\in G$ then applying $c$ will not move any cubelets. Reversing
    $c$, (applying $c'$) will only reorient the facelets, not permute
    any cubelets. Therefore, $\forall c\in C_o, \exists c' \in C_o$ such
    that $cc'=c'c = e$, as desired.

Because $C_o$ meets these criteria, we can confirm $C_o < G$. Now, we
know that a normal subgroup [@textbook] is such that
$\forall c \in C_o, g\in G$ $$gC_og' =\{gcg' |c\in C_o\} = C_o$$

From above we know that in order for $C_o \triangleleft G$,
$gcg' \in C_o, \forall c\in C_o, \forall g\in G$. This implies that
$gcg'$ cannot permute cubelets, but can alter the orientation of the
cubelets. Since, we do not care about how the cubelets are oriented, it
will be useful to write $g\in G$ and $c\in C_o$ as cubelet-level
permutations, so we can assess whether $gcg'\in C_o$. Let
$\theta\in S_{24}$ (for the 3x3 Rubik's Cube, or $\theta\in S_8$ for the
Pocket Cube) be the cubelet-level bijective map for all $g\in G$ and
$c \in C_o$. These will take the form $$g \mapsto g_C = \begin{bmatrix}
    1&2&...&24\\ \theta(1)&\theta(2)&...&\theta(24)
\end{bmatrix}$$ where $\theta(i)$ is the new cubicle occupied by the
cubelet leaving cubicle i. Without loss of generality for the ordering
of items: $$g'\mapsto g_C'=\begin{bmatrix}
    \theta(1)&\theta(2)&...&\theta(24) \\1&2&...&24
\end{bmatrix}$$ and at the cubelet-level, $g_Cg_C' = g_C'g_C  =e_C$.\
\
Notice $\forall c\in C_0$ $$c\mapsto c_C=\begin{bmatrix}
    1&2&...&24\\1&2&...&24
\end{bmatrix} = e_C$$ since $\forall c\in C_o$ they do not permute
cubelets, only orientations within cubicles. So, $\forall c\in C_o$,
$c = e$ or the identity at the cubelet level. Now, it is easy to see at
the cubelet level $\forall c\in C_o$ and $g\in G$
$$gcg' \mapsto g_Cc_Cg_C' = g_Ce_Cg_C' = g_Cg_C' = e_C$$ This means that
$gcg'$ does not permute any cubelets $\forall c\in C_o$ and
$\forall g\in G$. Therefore, $gcg' \in C_o$, implying
$C_o \triangleleft G$, as desired.

# The Fundamental Theorem of Cube Theory.

## Cube Orientation.

<figure id="fig:cube1" data-latex-placement="h">
<img src="../../../assets/images/Rubiks Cube/cube_orientation1.png" style="width:75.0%" />
<figcaption>Visualizing the 8 corner positions and 12 edge positions on
the 3x3 Rubik’s Cube <span class="citation"
data-cites="sfunotation"></span>.</figcaption>
</figure>

<figure id="fig:cube2" data-latex-placement="h">
<img src="../../../assets/images/Rubiks Cube/cube_orientation2.png" style="width:75.0%" />
<figcaption>Visualizing the orientation for the corner and edge facelets
<span class="citation" data-cites="sfunotation"></span>.</figcaption>
</figure>

Borrowing the orientation graphics for Mulholand's webpage (see Figure
[\[cube1\]](#cube1){reference-type="ref" reference="cube1"} and
[\[cube2\]](#cube2){reference-type="ref" reference="cube2"}), we can see
how we are labeling each cube. The corner cubicles are labeled 1 through
8. Relative to the user, the cubilcles are not mutable, only the cubelet
occupying that cubicle. For example, cubicle 3 will always be the
top-right cubicle to the user, but any of the 8 corner cubes may be
filling that position (see Figure [2](#fig:cube1){reference-type="ref"
reference="fig:cube1"}).The smae is done for the 12 edge cubicles.\
\
We also need to keep track of how each corner and edge piece are
oriented within their current cubicles. To facilitate this, we will need
two things:

1.  Label each facelet of each corner cube as follows: (a) in the solved
    position, the facelets Upper and Downward faces are labeled as the 0
    face, and (2) following each cube clockwise, label the other
    facelets 1 and 2, respectively.

So, when we discuss the orientation of the cube, each of the 3 possible
orientations of cubelet are described by which facelet is facing the
Upper or Downward face. For example, after move $F'$, the
yellow-green-red corner cube is now in cubicle 3, with orientation 2
(see Figure [4](#fig:moveB){reference-type="ref"
reference="fig:moveB"}).

<figure id="fig:moveB">
<div class="minipage">
<p><img src="../../../assets/images/Rubiks Cube/pocket_orientation.JPG" style="width:50.0%"
alt="image" /> <span id="fig:2x2cube"
data-label="fig:2x2cube"></span></p>
</div>
<div class="minipage">
<p><img src="../../../assets/images/Rubiks Cube/moveB_ex.JPG" style="width:50.0%" alt="image" /> <span
id="fig:moveB" data-label="fig:moveB"></span></p>
</div>
</figure>

## FTCT: 3x3 Rubik's Cube.

We can represent the state of our 3x3 Rubik's Cube using a 4-tuple,
$(\rho, \theta, v, w)$, where:

- $\rho\in S_8$, $\rho(i) = j$ represents corner $i$ in position $j$

- $\theta \in S_{12}$, $\theta(i) = j$ represents edge i in position j

- $v\in \mathbb{Z}^8_3$, represents the orientation of corners

- $w \in \mathbb{Z}_2^{12}$, represents the orientation of the edge
  pieces.

The 4-tuple $(\rho, \theta, v, w)$ corresponds to a **legal**
configuration of our Rubik's Cube if and only if The Fundamental Theorem
of Cube Theory (FTCT) for the 3x3 Rubik's Cube is
satisfied[@Bobzien][@sfucubology]. The FTCT has three criteria:

- Equal parity of permutations: $$sgn(\rho) = sgn(\theta)$$

- $v_1+v_2+...+v_8 \equiv  0$ (mod 3)

- $w_1+w_2+...+w_12 \equiv 0$ (mod 2)

In the FTCT, (1) says that there must be equal parity between the
permutations of the corners and edges (equal twists in orientation), (2)
is the conservation of corner twists: for every corner that flips
clockwise, another flips counter-clockwise, and (3) is the conservation
of the edge twists [@sweeney2022].\
\
To expand on the group theory we can represent the group of **All
Rubik's Cube positions** (legal and illegal) as

$$(\rho, \theta, v, w) \in S_8 \times S_12 \times \mathbb{Z}_3^8 \times\mathbb{Z}_2^{12}$$
which is the direct product between all these groups. Keep in mind the
symmetric groups that the corners and edge pieces constitute will come
in handy in the section on the solving algorithm and understanding how
it works.

## FTCT: Pocket Cube.

For the Pocket Cube, because we have no edge pieces to worry about, we
can represent our state with the 2-tuple,
$(\rho,v) \in S_8 \times \mathbb{Z}_3^8$ and corresponds to a **legal**
configuration of our Pocket Cube if and only if
$v_1+v_2+...+v_8 \equiv 0$ (mod 3). There are no edge pieces and parity
of permutations to worry about for The Pocket Cube.

## Cardinality.

Normally, calculating the order of a group is trivial for small, finite
groups because we can simply count the elements in our group. But, as we
will see, there is not enough time in a lifetime to count to $|G|$ for a
3x3 Rubik's Cube, so we need to get creative!\
\
Recall from the FTCT, we can represent the **Illegal Cube Group** as the
direct product of the groups
$$S_8\times  S_{12}\times \mathbb{Z}_3^8\times\mathbb{Z}_2^{12}$$ From
Lauritzen, we know that the order of a product group is given by
$G\times H = |G||H|$. Therefore,
$$|S_8\times  S_{12}\times \mathbb{Z}_3^8\times\mathbb{Z}_2^{12}| = |S_8||S_{12}||\mathbb{Z}_3^8||\mathbb{Z}_2^{12}| = (8!)(12!)(3^8)(2^{12})$$
which is the same as the number of all permutations of the Rubik's Cube
we found earlier, regardless of legality.

- **Group order for the 3x3 Rubik's Cube.** Using the FTCT, we can
  compute $|G|$. From the product group, $|\mathbb{Z}^8_{3}| = 3^8$, but
  the sum of the corner orientations, $v_1+...+v_8$, needs to be
  $\equiv 0$ (mod 3), which only happens for $\frac{1}{3}$ of all
  possible sums. Similarly, $|\mathbb{Z}_2^{12}| =2^{12}$, but the sum
  of the edge orientations, $w_1+w_2+..+w_{12} \equiv 0$ (mod 2), which
  only happens for $\frac{1}{2}$ of the sums. Finally,
  $sgn(\rho) = sgn(\theta)$, clearly, this only happens $\frac{1}{2}$ of
  the time as well. So, only
  $\frac{1}{3}(\frac{1}{2})(\frac{1}{2}) = \frac{1}{12}$ of the total
  permutations are legal [@Bobzien]. Using our result from earlier:
  $$|G| = \frac{1}{12}|S_8\times  S_{12}\times \mathbb{Z}_3^8\times\mathbb{Z}_2^{12}|=\frac{1}{12}(3^8(8!)(2^{12})(12!)) = 43,252,003,274,489,856,000 \approx 43x10^{19}$$
  Which is a pretty big (but finite) group! For a person counting
  elements in this group at a rate of 3 elements per second, it would
  take them over 400 billion years to count them all!

- **Group order for the Pocket Cube.** Again, we know we can represent
  the cube moves of the Pocket Cube as the direct product of the
  following groups:
  $$|S_8\times\mathbb{Z}_3^8|=|S_8||\mathbb{Z}_3^8| = (8!)(3^8)$$
  Because the Pocket Cube is essentially only the corners of the 3x3
  Rubik's Cube, only 7 of the corners can be rotated independently and
  we divide by 24 to account for the lack of fixed centers ($3(8) = 24$,
  where there are 3 orientations and 8 locations for the fixed cube)
  [@jaap]. $$|G| = \frac{1}{24}\cdot\frac{1}{3}(8!)(3^8) = 3,674,160$$
  possible configurations for the Pocket Cube-- quite a few less than
  the 3x3 Rubik's Cube, but still way too many to count!

- **Element & Subgroup orders.** Because we have shown previously that
  for any $g\in G$, $<g> \leq G$, we know that $|g| = |<g>|$.
  Furthermore, from Garron, we know that there are 73 possible orders of
  subgroups, ranging from 1 to 1260. The trivial subgroup has order 1,
  $|\{e\}| = 1$, and an example of an element with order 1260 is
  $|RFFB'U'B'| = 1260$ [@Garron].

# The 2 x 2 Rubik's Cube: The Pocket Cube

For sake of simplicity, we are going to focus our explorations on the
Pocket Cube as just the 3x3 Rubik's Cube's corners. This means we can
apply what we know about permuting the 3x3 Rubik's Cube's corners,
without having to worry about permutation any edge cubelets or center
cubelets. So, we will label our cubicles the same way (with respect to
the user) as in Figure [2](#fig:cube1){reference-type="ref"
reference="fig:cube1"}. When we talk about some of the group theory
(particularly cycles) behind this algorithm it will also be helpful to
refer to Figure
[\[fig:cubelayout\]](#fig:cubelayout){reference-type="ref"
reference="fig:cubelayout"} for the name of each facelet, as well as
Figure [5](#fig:2x2cube_ori){reference-type="ref"
reference="fig:2x2cube_ori"}, which shows a 3-D Pocket Cube with it's
facelets labeled.

<figure id="fig:2x2cube_ori">
<div class="minipage">
<p><img src="../../../assets/images/Rubiks Cube/CUBE LAYOUT2.png" style="width:100.0%" alt="image" />
<span id="fig:cubelayout" data-label="fig:cubelayout"></span></p>
</div>
<div class="minipage">
<p><img src="../../../assets/images/Rubiks Cube/pocket_orientation.JPG" style="width:50.0%"
alt="image" /> <span id="fig:2x2cube_ori"
data-label="fig:2x2cube_ori"></span></p>
</div>
</figure>

# How to Solve the Pocket Cube.

## The Quickest Method.

We will walk through the algorithm then discuss the group theory related
to why it works.

## Algorithm.

For the sake of clarity in this algorithm, we are going to assume that a
white facelet is occupying facelet 21. There is mention of something
called **commutators** and **conjugates** in this algorithm. These are
just moves that take a special form and have useful properties when
solving a Rubik's Cube. We will discuss how and way these work in a
subsequent section! For now, just bear with it through this algorithm:

1.  Regardless of orientation, find the cubelet we need to move into
    cubicle 7, and perform $U$ until it is in cubicle 3.

2.  Repeat the following commutator until the cubelet in cubicle 3 is
    oriented correctly in cubicle 7: $$[R,U] = RUR'U'$$

3.  Once complete, perform $D'$.

4.  Repeat steps 2-3 until the bottom layer (white side) is completely
    solved.

5.  On the top layer of cubelets, see if they are in the correct
    cubicle, regardless of orientation. There will either be 1, 2, or 4
    cubes in the correct cubicles. If there are 4, skip to step (8). If
    there are 1 or 2, perform the following commutator, performing $UD'$
    to get a correctly positioned cubelet into cubicle 4 (i.e., front
    left): $$[R,U'L'U] = RU'L'UR'U'LU$$

6.  Check again, performing $U$ if necessary to get 1 cubelet in the
    correct position.

7.  Repeat steps 5-6 until all 4 cubelets are in the correct cubicles,
    regardless of orientation.

8.  Flip the entire Pocket Cube the solved Down (white) face to the Up
    face. This is equivalent to performing: $$RRLL$$

9.  Since white is on our Up face, yellow will be our bottom face.
    Perform $D'$ to get an incorrectly orientated cubelet into
    cubicle 7. Then perform the commutator from (2) until it is
    correctly orientated: $$[R,U] = RUR'U'$$

10. Perform $D'$ to move the newly solved cubelet into cubicle 8.

11. Repeat steps 9-10 until the Down (yellow) face is completely solved.

12. Perform $U$ as needed to align the colors on the Front, Back, Left
    and Right Face.

13. Your Pocket Cube is now solved! [@youtube] [@youtube_solve]

## Why it works!

### Commutators.

Commutators are moves in $(G,\cdot)$ that take the form [@sweeney2022]
$$[A,B] = ABA'B'$$ As an example, let's examine the first commutator
from the 2x2 Algorithm: $$[R,U]=RUR'U'$$ There are only 2 faces that are
being permuted by $[R,U]$: $R$ and $U$. We move both, and then undo
both. These faces have one cubicle in common: cubicle 3 (see Figure
[6](#fig:randu){reference-type="ref" reference="fig:randu"}). But, then
performing $R'U'$, also permutes cubicles 1 and 2.If we perform $[R,U]$
again, cubicle 7 permutes with cubicle 3. So, we can see that if we
repeatedly perform the commutator $[R,U]$, cubicles 1 and 2 and 3 and 7
cycle and rotate, while the rest of the cube remains unchanged
[@sweeney2022].

<figure id="fig:randu" data-latex-placement="h">
<img src="../../../assets/images/Rubiks Cube/randu.png" style="width:100.0%" />
<figcaption>Cubes permuted by both <span
class="math inline">[<em>R</em>, <em>U</em>]</span>.</figcaption>
</figure>

In short, the first half of the commutator $[A,B] = ABA'B'$-- that is
$AB$ -- will overlap on some subset of the cublets, $K$. Then, when we
undo the moves in the second half -- $A'B'$ -- we change all the cublets
back to their original cubicle -- except for those in $K$. So, we can
effectively work with only a subset of the cublets, without effecting
the rest of the cube.

### Conjugates.

Conjugates in the Rubik's Cube world are exactly the same as in group
theory. The are moves that take the form: $ABA'$ where $A$ is a set up
move, and $A'$ the reverse of that set up move. These are useful for
using the same commutator to affect a different area of the cube, by
letting $B = [X,Y]$ where $[X,Y]$ is some commutator [@sweeney2022].

### Symmetric Groups & K-Cycles.

As touched on in the section on Normal Subgroups, the moves $g\in G$ can
be represented by bijective maps in the form of permutations.\
\
We can write this in more detail using symmetric groups and cycles. The
6 Basic Moves can be represented with the following cycles, using the
facelet notation [@textbook][@sfunotation]:

- $R = (13,14,16,15)(10,2,19,22)(12,4,17,24)$

- $L = (5,6,8,7)(3,11,23,18)(1,9,21,20)$

- $U = (1,2,4,3)(9,5,17,13)(10,6,18,14)$

- $D = (21,22,24,23)(11,15,19,7)(12,16,20,8)$

- $F = (9,10,12,11)(3,13,22,8)(4,15,21,6)$

- $B = (17,18,20,19)(1,7,24,14)(2,5,23,16)$

or cubelet-level notation

- $R = (2,6,7,3)$

- $L = (1,4,8,5)$

- $U = (1,2,4,3)$

- $D = (5,6,7,8)$

- $F = (3,7,8,4)$

- $B = (2,1,5,6)$

Using these cycles, we can use the Python SymPy library [@sympy]
(Appendix A & B) to calculate the cycles in terms of facelets or just
cubelets:
$$(1) [R,U] = RUR'U' = (1, 14, 18, 17, 5, 2)(4, 15, 13, 22, 10, 12)$$ or
$$(2)[R,U] = RUR'U' = (1,2)(3,7) = (1,2)(3,4)(4,5)(5,6)(6,7)(5,6)(4,5)(3,4)$$

So, we can see from (2) using this commutator cubicles 1 and 2
continually switch cubelets, and so do cubicles 3 and 7 (2-cycles). We
can also see from (1) how the facelets swap as the cubicles switch
location and rotate in 6-cycles, and also that all the facelet positions
in the first cycle of (1) are on cubicles 1 and 2 while all the facelet
positions in the second cycle are on cubicles 3 and 7. It is also
important to notice that cubicles 4,5,6, and 8 and their respective
facelets are completely unaffected by this cycle-- which is of course
useful when trying to solve one layer of the Pocket Cube at a time.\
\
The facelet notation is also useful to the user when trying to figure
out how many times to perform $[R,U]$ to correctly position their needed
facelet. For example, if the user is trying to correctly position the
facelet in facelet 15, they need to perform $[R,U]^2$. If the facelet is
in facelet 12, they need to perform $[R,U]^4$.\
\
From Lauritzen, we also know that $n(\sigma)$ is the number of simple
transpositions of which $\sigma$ is a product, where the simple
transposition is of the form $s = (i\text{   }i+1)$. So, from (2)
$$n([R,U]) = 8$$ and $$sgn([R,U]) = (-1)^{n([R,U])} = +1$$ Therefore
$[R,U]$ is an even permutation. Because $[R,U]$ is a legal move. And, by
Lauritzen we know
$$|[R,U]| =|(1, 14, 18, 17, 5, 2)(4, 15, 13, 22, 10, 12)| =lcm(6,6) = 6$$
Meaning this commutator takes 6 repetitions (i.e., $[R,U]^6 = e_G$) to
get back the the start configuration.\
\
We can do the same thing for our other commutator $[R, U'L'U ]$:
$$(3) [R,U'L'U] = (2,3)(7,8)$$
$$(4) [R, U'L'U] = (1,13,14)(2,5,10)(4,17,18)$$ Again, in terms of
cubelets, it is the product of two 2-cycles. When looking at the
facelets we have the product of three 3-cycles. So, cubicles 2 and 3,
and 7 and 8 are continually swapping cubelets and reorienting the
cubelet within the cubicle each cycle. Also,
$$|[R,U'L'U]| = |(1,13,14)(2,5,10)(4,17,18)|=lcm(3,3,3) = 3$$ SO it only
takes 3 repetitions to get back to the start configuration (i.e.,
$[R, U'L'U]^3 = e_G$). Since $(2,3)(7,8)$ are already simple
transpositions $$n([R,U'L'U]) = 2$$
$$sgn([R,U'L'U]) = (-1)^{n([R,U'L'U])} = +1$$ so it is an even
permutation.

# How to Make an Impossible Rubik's Cube

Finally, we have all the information we need to make our impossible
Rubik's Cubes. All we need to do is perform an illegal move (obviously
on a physical Rubik's Cube we cannot do this without removing and
rearranging the cubelets or the stickers on the facelets, but suspend
your disbelief here), and then all solving algorithms will fail. All our
illegal move needs to do is violate any one of the three criteria in the
3x3 Rubik's Cubes FTCT. Any Move that violates the $v$ criteria is also
an illegal move for the Pocket Cube. So, some illegal moves we could use
to make our impossible Rubik's Cube is:

1.  Rotate any 1 corner cube.

2.  Rotate any 1 edge cube.

3.  Swap any 2 adjacent cubes.

To begin, let's examine and verify that $[R,U]$ from the solving
algorithm is legal move and justify it. Looking at the Pocket Cube FTCT,
the only criteria we need to meet is
$$v_1+v_2+...+v_2 \equiv  0 \text{ (mod 3)}$$ From Figures
[\[fig:ru_up\]](#fig:ru_up){reference-type="ref" reference="fig:ru_up"}
and [7](#fig:ru_d){reference-type="ref" reference="fig:ru_d"} we can
calculate this quantity.

<figure id="fig:ru_d">
<div class="minipage">
<p><img src="../../../assets/images/Rubiks Cube/ru_up.JPG" style="width:50.0%" alt="image" /> <span
id="fig:ru_up" data-label="fig:ru_up"></span></p>
</div>
<div class="minipage">
<p><img src="../../../assets/images/Rubiks Cube/ru_down.JPG" style="width:50.0%" alt="image" /> <span
id="fig:ru_d" data-label="fig:ru_d"></span></p>
</div>
</figure>

Summing from cubicle 1 to 8:
$$v_1+v_2+...+v_2 \equiv 0 \text{ (mod 3)} \equiv 0+1+1+0+0+0+1+0\text{ (mod 3)} \equiv 3\text{ (mod 3)} \equiv 0$$
So, we can tell this is a legal move on the Pocket Cube via the Pocket's
Cube FTCT.

Now, examining the illegal moves, Illegal Move (1) Rotating any one
corner cube is a very simple option. Without loss of generality to all
legal positions, consider the solved Rubik's Cube configuration. Here,
we have
$$v_1+v_2+...+v_2 \equiv 0 \text{ (mod 3)} \equiv 0+0+0+0+0+0+0+0\text{ (mod 3)} \equiv 0$$
Rotating any one corner cube would result in, without loss of generality
to the cubicle of the corner we're working with
$$v_1+v_2+...+v_2 \equiv 0 \text{ (mod 3)} \equiv 1+0+0+0+0+0+0+0\text{ (mod 3)}\equiv 1\text{ (mod 3)} \not \equiv 0$$
or
$$v_1+v_2+...+v_2 \equiv 0 \text{ (mod 3)} \equiv 2+0+0+0+0+0+0+0\text{ (mod 3)} \equiv 2\text{ (mod 3)} \not \equiv 0$$
Clearly, this violates the conservation of corner rotations. Rotating
more than 1 corner might accidentally result in a sum that is congruent
to 0 (mod 3), so best to only rotate one! Illegal Move (2) works the
same way, but instead violates the conservation of edge flips, and is
only applicable to the 3x3 Rubik's Cube.

Illegal Move (3) is a little more interesting, but is only applicable to
the 3x3 Cube. Obviously, we cannot swap a corner with an edge cube,
because they have a different number of facelets. Let's find $\rho$ and
$\theta$ in disjoint cycle notation, in the case where the 2 pieces
swapped are corner cubes in adjacent cubicles $i$ and $i+1$, clearly:
$$\rho = (i,i+1)$$ which consists of 1 simple transposition. Because no
edge pieces are involved in this swap we have
$$\theta = (1)(2)(3)(4)(5)(6)(7)(8)(9)(10)(11)(12)$$ which has 0 simple
transpositions. Therefore, we have

$$sgn(\rho) \not= sgn(\theta)$$ $$(-1)^{n(\rho)}\not=(-1)^{n(\theta)}$$
$$(-1)^{1}\not=(-1)^{0}$$ $$-1 \not = 1$$ which violates the FTCT
criteria on Equal Parity. Therefore, this is an illegal move and will
render the Rubik's Cube unsolvable.\
\
So, if we are handed a Rubik's Cube, by simply finding the 4-tuple
representing it's configuration we can easily determine if it is
solvable! Conversely, we can easily make our own trick Illegal Rubik's
Cubes using the same group theory!

# Bibliography.

:::::: appendices
All code is available here:
<https://github.com/Sara-Pesso/rubiks-cube-group-theory>.

# Rubik's Cube Permutations Script

::: spverbatim
from sympy.combinatorics.permutations import Permutation

\# Define permutations using array notation (0-indexed) \# SymPy indexes
from 0, so we put 0 to nonexistent 0-cubicle \# so it doesn't effect or
calculations or notation in our paper

\# Cubicle Permutations (disregards cube orientation within cubicle) R =
Permutation(2,6,7,3) U = Permutation(1,2,3,4) L = Permutation(1,4,8,5) D
= Permutation(8,7,6,5) B = Permutation(2,1,5,6) F = Permutation(3,7,8,4)

R_inv =  R U_inv =  U L_inv =  L D_inv =  D B_inv =  B F_inv =  F

\# Facelet Permutations: Using Jamie Mulholand's notation
(https://www.sfu.ca/ jtmulhol/math302/puzzles-rc-representation.html)

Rf = Permutation(13,14,16,15)(10,2,19,22)(12,4,17,24) Uf =
Permutation(1,2,4,3)(9,5,17,13)(10,6,18,14) Lf =
Permutation(5,6,8,7)(3,11,23,18)(1,9,21,20) Df =
Permutation(21,22,24,23)(11,15,19,7)(12,16,20,8) Bf =
Permutation(17,18,20,19)(1,7,24,14)(2,5,23,16) Ff =
Permutation(9,10,12,11)(3,13,22,8)(4,15,21,6)

Rf_inv =  Rf Uf_inv =  Uf Lf_inv =  Lf Df_inv =  Df Ff_inv =  Ff
:::

# Permutation Exploration Script

::: spverbatim
from rubiks_cube_permutations import \* \# Group Theory Operations! \#
Commutator RUR'U' commutator = R\*U\*R_inv\*U_inv print(\"Commutator:
\[R,U\] = RUR'U'\") print(\"Cubicle notation:\") print(f\"Commutator:
commutator\") \# Permutation multiplication print(f\"Order:
commutator.order()\") \# Order of the permutation print(f\"Cycle Form:
commutator.cyclic_form\") \# Cycle notation print(f\"Permutation Form:
commutator.array_form\") \# Permutaion notation print(f\"Sign:
commutator.signature()\") \# Sign of permutation

print(\"Facelet notation:\") commutator = Rf\*Uf\*Rf_inv\*Uf_inv
print(f\"Commutator: commutator\") \# Permutation multiplication
print(f\"Order: commutator.order()\") \# Order of the permutation
print(f\"Cycle Form: commutator.cyclic_form\") \# Cycle notation
print(f\"Permutation Form: commutator.array_form\") \# Permutaion
notation print(f\"Sign: commutator.signature()\") \# Sign of permutation

\# Commutator \[R, U 'L'U \] = RU 'L'U R'U 'LU commutator =
R\*U_inv\*L_inv\*U\*R_inv\*U_inv\*L\*U print(\"Commutator: \[R, U'L'U \]
= RU'L'UR'U'LU\") print(\"Cubicle notation:\") print(f\"Commutator:
commutator\") \# Permutation multiplication print(f\"Order:
commutator.order()\") \# Order of the permutation print(f\"Cycle Form:
commutator.cyclic_form\") \# Cycle notation print(f\"Permutation Form:
commutator.array_form\") \# Permutaion notation print(f\"Simple
Transpositions: commutator.transpositions()\") print(f\"Sign:
commutator.signature()\") \# Sign of permutation

print(\"Facelet notation:\") commutator =
Rf\*Uf_inv\*Lf_inv\*Uf\*Rf_inv\*Uf_inv\*Lf\*Uf print(f\"Commutator:
commutator\") \# Permutation multiplication print(f\"Order:
commutator.order()\") \# Order of the permutation print(f\"Cycle Form:
commutator.cyclic_form\") \# Cycle notation print(f\"Permutation Form:
commutator.array_form\") \# Permutaion notation print(f\"Simple
Transpositions: commutator.transpositions()\") print(f\"Sign:
commutator.signature()\") \# Sign of permutation
:::

**Output:**

::: spverbatim
Commutator: \[R,U\] = RUR'U' Cubicle notation: Commutator: (1 2)(3 7)
Order: 2 Cycle Form: \[\[1, 2\], \[3, 7\]\] Permutation Form: \[0, 2, 1,
7, 4, 5, 6, 3\] Sign: 1 Facelet notation: Commutator: (24)(1 14 18 17 5
2)(4 15 13 22 10 12) Order: 6 Cycle Form: \[\[1, 14, 18, 17, 5, 2\],
\[4, 15, 13, 22, 10, 12\]\] Permutation Form: \[0, 14, 1, 3, 15, 2, 6,
7, 8, 9, 12, 11, 4, 22, 18, 13, 16, 5, 17, 19, 20, 21, 10, 23, 24\]
Sign: 1 Commutator: \[R, U'L'U \] = RU'L'UR'U'LU Cubicle notation:
Commutator: (8)(1 3 2) Order: 3 Cycle Form: \[\[1, 3, 2\]\] Permutation
Form: \[0, 3, 1, 2, 4, 5, 6, 7, 8\] Simple Transpositions: \[(1, 2), (1,
3)\] Sign: 1 Facelet notation: Commutator: (24)(1 13 14)(2 5 10)(4 17
18) Order: 3 Cycle Form: \[\[1, 13, 14\], \[2, 5, 10\], \[4, 17, 18\]\]
Permutation Form: \[0, 13, 5, 3, 17, 10, 6, 7, 8, 9, 2, 11, 12, 14, 1,
15, 16, 18, 4, 19, 20, 21, 22, 23, 24\] Simple Transpositions: \[(1,
14), (1, 13), (2, 10), (2, 5), (4, 18), (4, 17)\] Sign: 1
:::
::::::
