---
layout: publication
title: "Generating proof systems for three-valued propositional logics"
where: Coming soon as a book chapter
date: 2023-06-01
authors: V. Greati, G. Greco, S. Marcelino, A. Palmigiano, U. Rivieccio
preprint: "www.arxiv.org/abs/2401.03274"
tags: [three-valued logics, many-valued logics, Hilbert systems, multiple-conclusion logics]
---

In general, providing an axiomatization for an arbitrary logic is a task that
may require some ingenuity. In the case of logics defined by a finite logical matrix 
(three-valued logics being a particularly simple example), the generation
of suitable finite axiomatizations can be completely automatized, essentially
by expressing the matrix tables via inference rules. In this chapter we illustrate 
how two formalisms, the 3-labelled calculi of Baaz, Fermüller and Zach
and the multiple-conclusion (or Set-Set) Hilbert-style calculi of Shoesmith
and Smiley, may be uniformly employed to axiomatize logics defined by a
three-valued logical matrix. We discuss their main properties (related to completeness, 
decidability and proof search) and make a systematic comparison
between both formalisms. We observe that either of the following strategies
are pursued: (i) expanding the metalanguage of the formalism (via labels or
types) or (ii) generalizing the usual notion of subformula property (as done
in recent work by C. Caleiro and S. Marcelino) while remaining as close as
possible to the original language of the logic. In both cases, desirable requirements 
are to guarantee the decidability of the associated symbolic procedure,
as well as the possibility of an effective proof search. The generating procedure 
common to both formalisms can be described as follows: first (i) convert
the matrix semantics into rule form (we refer to this step as the generating
subprocedure) and then (ii) simplify the set of rules thus obtained, essentially
relying on the defining properties of any Tarskian consequence relation (we
refer to this step as the streamlining subprocedure). We illustrate through
some examples that, if a minimal expressiveness assumption is met (namely,
if the matrix defining the logic is monadic), then it is straightforward to define
effective translations guaranteeing the equivalence between the 3-labelled and
the Set-Set approach.
