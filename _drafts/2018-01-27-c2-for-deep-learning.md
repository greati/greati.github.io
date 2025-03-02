---
title: The Multi-variable Calculus you need to master Deep Learning mathematically
---

<p>Deep Learning is the field of using Artificial Neural Networks (ANN) with more than two layers
for solving a plethora of problems,
which is now feasible thank to the advances in hardware performance.
New applications and techniques in this field started to fervently
emerge and learning Deep Learning (sorry) today is a must for Computer Science 
students and professionals.</p>

<br/>

<p>
The subject can be taught in two main ways: (a) hiding the Mathematics behind the
neural networks operation, teaching just how to implement and use them; or (b) 
spending some time on the main equations
that govern neural networks in order to build a solid understanding of the technique.
Of course, (b) achieves (a) naturally with very low effort and is much more beautiful,
interesting and funny.
</p>

<br/>

<p>
A very impressive text which teaches the Mathematics of Deep Learning is the
second chapter of Michael Nielsen's book, 
<a target="_blank" href="http://neuralnetworksanddeeplearning.com/chap2.html">
Neural Networks and Deep Learning</a>. The purpose of this article is to 
present the basics of Multi-variable Calculus needed to 
prove the four equations that perform the backpropagation algorithm, the engine of
Deep Learning's neural networks.
The main reference was the fabulous book "Calculus, Volume II", 
by Apostol. Actually, all definitions and theorems are there, 
in almost the same way, but I included comments which, for me, ease
the understanding.
</p>

<br/>

<p>
Nielsen says that the <strong>chain rule</strong> is enough to prove the equations. Until getting there,
some preliminaries definitions and results  are necessary. 
Some are unnecessary too, but I do not guarantee I will
resist to put them here.
</p>

<br/>

<p>
Let's start by defining <strong>scalar fields</strong>, since learning is a process
of optimizing a scalar field:
</p>

<br/>

<p>
Definition (Scalar field): A function

\[ C:\mathbb{R}^n \to \mathbb{R}  \]

is a scalar field when \(n > 1\). It is also called a real-valued function of a vector variable.
</p>

<br/>

<p>
For now on, let's work with the scalar field \( C \) defined above.
Optimizing \( C \) is the task of taking a \( x \in X \subset \mathbb{R}^n \) for
which \(C(x)\) is minimum (or maximum, depending on the problem) in \(X\). Finding this \( x \),
at least in backpropagation algorithm, demands walking through a sequence  \( \{x_i\}_{i=0}^k \) of
points in \(X\) for which the change from \( C(x_i)\) to \( C(x_{i+1}), i=0,\ldots,k-1\), is interesting for
the optimization. This is possible when we understand how \(C\) changes when its argument changes,
and here is where the <strong>derivative</strong> becomes important. Since derivatives are
limits, we need the definition of limits for scalar fields. Before that, let's define some
important entities.
</p>

<br/>

<p>
Definition (Open ball): An open \(n\)-ball of center \( a \in \mathbb{R}^n \) and radius \( r > 0 \) is the set
\[ B(a;r) = \{ x \in \mathbb{R}^n \mid ||x - a|| < r \}.\]
</p>

<br/>

<p>
Definition (Interior point): Let \( S \subset \mathbb{R}^n \) and \( a \in \mathbb{R}^n \).
If there exists an open \(n\)-ball centered at \(a\) for which all of its points are 
in \(S\), \(a\) is called an interior point of \(S\). The set of all interior points
of \(S\) is the interior of \(S\), denoted by \(int(S)\).
</p>

<br/>

<p>
Definition (Open set): \(S \subset \mathbb{R}^n\) is an open set if 
all its points are interior points, i.e, if and only if \(S = int(S)\).
</p>

<br/>

<p>
Definition (Exterior point): \(x \in \mathbb{R}^n\) is exterior to a subset \(S\)
if there is an \(n\)-ball \(B(x)\) containing no points of \(S\). 
The set of all exterior points of \(S\) is denoted by \(ext(S)\).
</p>

<br/>

<p>
Definition (Boundary): The set of points which are neither interior nor
exterior to \(S\) is called the boundary of \(S\) and is denoted by
\(\partial S\).
</p>

<br/>

<p>
Now it is possible to define limits and continuity!
</p>

<br/>

<p>
Definition (Limit): For \( a \in \mathbb{R}^n\) and \( b \in \mathbb{R}^m\), we write
\[\lim_{x \to a} C(x) = b\]
to denote that
\[ \lim_{||x - a|| \to 0} ||C(x)-b|| = 0\]
in the usual elementary calculus. In other words, if \( h = x-a\), 
\[ \lim_{||h|| \to 0}||C(a+h) - b|| = 0.\]
</p>

<br/>

<p>
Definition (Continuity): \(C\) is continuous at a point \(a \in \mathbb{R}^n\) if
\[\lim_{x \to a} C(x) = C(a).\]
\(C\) is continuous on a set \(S\) if it is continuous at every point in \(S\).
</p>

<br/>

<p>
Now we can move on to derivatives. In single variable calculus, the derivative 
of a function \(f : \mathbb{R} \to \mathbb{R} \)
is built upon the incremental quotient:
\begin{equation}
	\frac{f(b)-f(a)}{b-a} = \frac{f(a+h)-f(a)}{h}, h=b-a.
\end{equation}
This quotient reflects the <strong>average rate of change</strong> 
when the function moves from \(a\) to \(b\). Notice that, in this case,
this movement can happen only in two ways: move from \(a\) to the left
or to the right by some amount, which can be expressed by a real number,
whose sign determines the sense of the movement. In other words, there is
only one direction.
</p>

<br/>

<p>
For the multivariable calculus, we can get some intuition by taking
a scalar field \(f : S \subset \mathbb{R}^2 \to \mathbb{R} \). Taking some interior point
\( a \in S\), how can we move? Differently from the single variable
case, there are infinitely many directions, so a simple real number is not enough to
characterize a displacement from \( a \). We need to indicate the direction,
and this can be done using a vector, say \(y \in \mathbb{R}^2 \). However,
how much do we want to move? Since \( y \) is a vector, doing \(a + y\)
causes a movement of magnitude \(||y||\) in the sense of \(y\), so another
parameter is required, such that we preserve the information about the direction,
but change the sense and the magnitude of the movement. From Linear Algebra, multiplying
a vector by a scalar is what we need. Call this scalar \( h \in \mathbb{R} \). Therefore,
\[ a + hy \]
expresses the change from \(a\) in the direction and sense of vector \(hy\) by an amount
\( |h|\cdot||y||\). Since \(a \in int(S)\), there is an \(n\)-ball \(B(a;r) \subset S\).
Taking \(h\) such that \( |h|||y|| < r \), the segment from \(a\) to \( a + hy\) 
lye intire in \(S\). Now, we form the difference

\begin{equation}
	\frac{f(a+hy)-f(a)}{h}
\end{equation}

which is the average rate of change over the line joining \(a\) and \(a + hy\).
Since we want such change when \(h \to 0\), we finally reach the definition
of derivative with respect to a vector. It is important to remember that
all the definition below works for the \(\mathbb{R}^n\) case.
</p>

<br/>

<p>
Definition (Derivative of a scalar field with respect to a vector):
Given a scalar field \(f : S \subset \mathbb{R}^n \to \mathbb{R} \), 
an interior point \( a \in S\) and any \(y \in \mathbb{R}^n\),
the <strong>derivative of \(f\) at \(a\) with respect to \(y\)</strong> is
defined by
\begin{equation}
	f'(a;y)=\lim_{h \to 0}\frac{f(a+hy)-f(a)}{h}
\end{equation}
when such limit exists.
</p>

<br/>

<p>
What if \(y\) is a <strong>unit vector</strong>?
</p>

<br/>

<p>
Definition (Directional derivative): If \(y\) is a unit vector, then \(f'(a;y)\) is called
the directional derivative of \(f\) at \(a\) in the direction of \(y\).
</p>

<br/>

<p>
What are important unit vectors? Well, the set of vectors \( \mathbb{B} = \{e_1, e_2, \ldots, e_n\}\),
where each \(e_k\) has 1 at the \(k^{th}\) component, and 0 at the others. Directional derivatives
with respect to these vectors are <strong>partial derivatives</strong>.
</p>

<br/>

<p>
Definition (Partial derivatives): If \(y=e_k\), the directional derivative \(f'(a;e_k)\) is called
the partial derivative with respect to \(e_k\), being also denote by \(D_kf(a)\):
\[D_kf(a) = f'(a;e_k).\]
The same notion can be denoted by:
\[D_kf(a_1, \ldots, a_n)\]
\[\frac{\partial f}{\partial x_k}(a_1, \ldots, a_n)\]
\[f'_{x_k}(a_1, \ldots, a_n)\]
\[f_{x_k}(a_1, \ldots, a_n).\]
</p>

<br/>

<p>
Of course, if we take the partial derivative of a partial derivative, we get
a partial derivative of second order. Keeping taking more partial derivatives,
we achieve higher orders.
</p>

<br/>

<p>
Actually, directional derivatives are a somewhat unsatisfactory extension of the one-dimensional
concept of derivative. A more suitable generalization implies continuity and allows
extending the principal theorems of one-dimensional derivative theory to the higher 
dimensional case. <strong>Total derivatives</strong> are what we are looking for.
</p>

<br/>

<p>
For reaching this definition, it is important to remember that a function \(f\)
with derivative at \(a\) can be approximated by a <strong>linear Taylor polynomial</strong>:
\[ f(x) \approx f(a) + f'(a)(x-a) \]
or, by writing \( h = x-a\),
\[ f(h+a) \approx f(a) + hf'(a). \]
</p>

<br/>

<p>
Given that \(f'(a)\) exists, define \(E(a,h)\) by
\[ E(a,h) = \frac{f(a+h)-f(a)}{h} - f'(a) \qquad h \neq 0\]
with \(E(a,0) = 0\). Manipulating that,
we get
\begin{equation}
\label{eq:taylorfirst}
f(a+h)=f(a)+hf'(a)+hE(a,h).
\end{equation}
Notice that the equality holds, because \(E(a,h)\) expresses the error
of the approximation. Also, as \(h \to 0\), \(E(a,h) \to 0\).
This property of approximating a differentiable function by a linear function
suggests that the concept of differentiability can be extended to the higher dimensional
case.
</p>

<br/>

<p>
Definition (Total derivative): Let \(f:S \subset \mathbb{R}^n \to \mathbb{R}^m\), 
\(a \in int(S)\) and \(B(a;r) \subset S\). Also, let \(\mathbb{v} \in \mathbb{R}^n\)
with \(||v|| < r\), such that \(a + \mathbb{v} \in B(a;r)\). We say that <strong>\(f\) is
differentiable at \(a\)</strong> if there exists a linear transformation
\[T_a:\mathbb{R}^n \to \mathbb{R}\]
and a scalar function \(E(a,\mathbb{v})\) such that
\begin{equation}
\label{eq:taylorhigher}
f(a+\mathbb{v}) = f(a) + T_a(\mathbb{v}) + ||\mathbb{v}||E(a,\mathbb{v})
\end{equation}
for \(||v|| < r\), where \(E(a,v) \to 0\) as \(||v|| \to 0\). The linear transformation
\(T_a\) is the <strong>total derivative</strong> of \(f\) at \(a\).
</p>

<br/>

<p>
Equation \ref{eq:taylorhigher} is the <strong>first-order Taylor formula for \(f(a+v)\)</strong>.
It gives an approximation to the difference \(f(a+v)-f(v)\) by \(T_a(v)\) with error
\(||v||E(a,v)\), for which \(E(a,v) \in o(||v||)\) as \(||v|| \to 0\). The following
theorem makes useful this definition.
</p>

<br/>

<p>
Theorem: Let \(f\) be differentiable at \(a\), and call \(T_a\) its total
derivative. Then the derivative \(f'(a;y)\) exists for every \(y \in \mathbb{R}^n\)
and 
\begin{equation}
	T_a(y) = f'(a;y)
\end{equation}
Besides, \(f'(a;y)\) is a linear combination of the components of \(y\):
\begin{equation}
\label{eq:ycomb}
	f'(a;y) = \sum\limits_{k=1}^{n}\frac{\partial f}{\partial e_k}(a)y_k
\end{equation}
</p>

<br/>

<p>
Thanks to the concept of total derivative, we reach the very important
Equation \ref{eq:grad}, which motivates the introduction of a vector 
which appears in various Computer Science's applications, 
such that image edge segmentation (Image Processing) and
gradient descent (Neural Networks): the <strong>gradient vector</strong>.
</p>

<br/>

<p>
Definition (Gradient vector): The gradient vector \(\nabla f(a)\) 
(the gradient of \(f\) at \(a\)) is a vector
whose components are the partial derivatives of \(f\) at \(a\):
\begin{equation}
\label{eq:grad}
	\nabla f(a) = \left\langle \frac{\partial f}{\partial e_1}(a), \frac{\partial f}{\partial e_2}(a),
 \ldots,\frac{\partial f}{\partial e_n}(a)\right\rangle
\end{equation}

In this way, it is possible to rewrite Equation \ref{eq:ycomb} as a dot product:
\[f'(a;y) = \nabla f(a) \cdot y.\] 

Also, we can make Equation \ref{eq:taylorhigher} like Equation \ref{eq:taylorfirst} (thus
making \(T_a\) to play the role of \(f'(a)\)):

\begin{equation}
f(a+v) = f(a) + \nabla f(a) \cdot v + ||v||E(a,v).
\end{equation}

This Taylor formula allows to show that if a scalar field is differentiable at \(a\), then it is continuous
at \(a\). Also, it is possible to show that if all partial derivatives exist in some \(n\)-ball
\(B(a)\) and are continuous at \(a\), then \(f\) is differentiable at \(a\).
</p>

<br/>

<p>
Finally, we can get to the <strong>chain rule</strong>. It is worth remembering 
that for single variable calculus,
if \(g(t) = f(r(t))\), then
\[g'(t) = f'(r(t))r'(t).\]
For scalar fields, we have a theorem which expresses a chain rule.
</p>

<br/>

<p>
Theorem (Chain rule): Let \(f\) be a scalar field defined on an open set \(S\) in \(\mathbb{R}^n\),
and let \(r\) be a vector-valued function which maps an interval \(J\) from \(\mathbb{R}\) into \(S\).
Define the composite \(g=r;f\) on \(J\) by
\[g(t) = f[r(t)].\]
Let \(t\) be a point in \(J\) at which \(r'(t)\) exists and assume that \(f\) is differentiable
at \(r(t)\). Then \(g'(t)\) exists and is equal to the dot product

\begin{equation}
\label{eq:chainrule}
	g'(t) = \nabla f(r(t)) \cdot r'(t).
\end{equation}
</p>

<br/>

<p>
Of course, I couldn't prove most of the equations I put here, but everything
you need to understand and prove the four main equations of 
Neural Network is here. Actually, Equation \ref{eq:chainrule} 
maybe is just enough, but, well, I couldn't let the beautiful piece of
Mathematics that makes all of that so precise and correct out of this.
</p>

