---
title: Automatic soundness check of sequent-style (and Hilbert-style) rules
---

I present in this post a tool to check soundness of sequent-style rules
(and Hilbert-style, in particular) with respect to a family of (deterministic) logical matrices
over the same algebra (i.e., it allows to specify multiple sets of designated values).
In order to run the tool, you only need to install <a target="_blank" href="https://www.docker.com/">Docker</a>.
The logical matrix and the rules to be checked are specified in an yaml file.
The file is then submitted to the tool by a command-line interface.

Let's go step by step.

#### What is soundness?
Let $$\mathcal{M} := \{ \langle \mathbf{A},D_i \rangle \}_{i \in I}$$ be a family of logical matrices.
Recall that a sequent $$\Gamma \Rightarrow \Delta$$ is satisfied in $$\mathcal{M}$$
by an $$\mathbf{A}$$-valuation $$v$$ when it is not the case that, for some $$i \in I$$, $$v[\Gamma] \subseteq D_i$$ and
$$v[\Delta] \subseteq A{\setminus}D_i$$.
A sequent rule is sound in $$\mathcal{M}$$ when, for every $$\mathbf{A}$$-valuation $$v$$,
it is not the case that the premises are satisfied in $$\mathcal{M}$$ by $$v$$ and the conclusion is not.


#### Basic setup
1. Install Docker in your machine.
2. Have a local copy of the repository available at <a target="_blank" href="https://github.com/greati/logicantsy">https://github.com/greati/logicantsy</a>
(you can simply clone it).
3. Build the image based on the Dockerfile available in the repository. A common way is to run the command:
`docker build -t logicantsy .` inside the folder.

#### Specify your logical matrix

Let's specify the family $$\{ \langle \mathbf{PP_6, {\uparrow}{\mathbf f}} \rangle, \langle \mathbf{PP_6, {\uparrow}{\mathbf b}} \rangle,
 \langle \mathbf{PP_6, {\uparrow}{\mathbf{\hat t}}} \rangle\}$$
of 6-valued matrices, where $$\mathbf{PP_6}$$ is the algebra given <a target="_blank" href="https://arxiv.org/pdf/2309.06764">here</a>.
The yaml is listed below (after it I give an explanation).

```
pnmatrix:
  values: [^f, f, n, b, t, ^t]
  distinguished_sets_structure:
    up_t_hat:
      - [^t]
    up_b:
      - [b, t, ^t]
    up_f:
      - [f, b, n, t, ^t]
  interpretation:
    p -> q:
      default: [^t]
      restrictions:
        - [_,  ^f]: [^f]
        - [^f, ^f]: [^t]
        - [n, f]: [b]
        - [b, f]: [n]
        - [t, f]: [f]
        - [^t, f]: [f]
        - [b, n]: [n]
        - [t, n]: [n]
        - [^t, n]: [n]
        - [n, b]: [b]
        - [t, b]: [b]
        - [^t, b]: [b]
        - [^t, t]: [t]
    p or q:
      restrictions:
        - [^f, ^f]: [^f]
        - [f, _]  : [f]
        - [_, f]  : [f]
        - [n, _]  : [n]
        - [_, n]  : [n]
        - [b, _]  : [b]
        - [_, b]  : [b]
        - [b, n]  : [t]
        - [n, b]  : [t]
        - [t, _]  : [t]
        - [_, t]  : [t]
        - [^t, _] : [^t]
        - [_, ^t] : [^t]
    p and q:
      restrictions:
        - [^t, ^t]: [^t]
        - [t, _]  : [t]
        - [_, t]  : [t]
        - [b, _]  : [b]
        - [_, b]  : [b]
        - [n, _]  : [n]
        - [_, n]  : [n]
        - [n, b]  : [f]
        - [b, n]  : [f]
        - [f, _]  : [f]
        - [_, f]  : [f]
        - [^f, _] : [^f]
        - [_, ^f] : [^f]
    neg p:
      restrictions:
        - [^f]: [^t]
        - [f] : [t]
        - [n] : [n]
        - [b] : [b]
        - [t] : [f]
        - [^t]: [^f]
    o(p):
      restrictions:
        - [^f]: [^t]
        - [f] : [^f]
        - [n] : [^f]
        - [b] : [^f]
        - [t] : [^f]
        - [^t]: [^t]
    bot():
      restrictions:
        - []: [^f]
    top():
      restrictions:
        - []: [^t]
sequent_dset_correspondence: [0, 1]
max_counter_models: 10
rules:
  r1:
    premises:
      - [["p"],["q"]]
      - [["r"],["s"]]
    conclusions:
      - [["p","q -> r"],["s"]]
  r2: 
    premises:
      - [["q"],["p"]]
      - [["q"],["neg p"]]
    conclusions:
      - [["q"],["bot()"]]
```

This yaml description has three parts.

The first one, `pnmatrix`, is the specification of the matrix. 
First we list the values, then the distinguished sets (note that `up_t_hat`, `up_b` and `up_f` are names
the user can control), and then the interpretation of the connectives.
The reader is referred to Appendix A of <a target="_blank" href="/texts/MsCVitorGreati.pdf">this document</a>
for a detailed explanation of how to specify the interpretations.

The second part are configuration parameters given to the program.
The user may care only about `max_counter_models: 10`, which
says what is the maximum of countermodels the program will output in case a rule is not sound.

Finally, the third part list the rules we want to check.
A rule is specified by a name (in this case, `r1` and `r2`),
and then by the list of premises and the conclusion.
For premises, one lists each premise in a separate line.
The format for a premise and for a conclusion is a pair of lists of formulas.
In the example, I'm asking to check soundness of the rules

$$
\frac{p \Rightarrow q \qquad r \Rightarrow s}{p,q \to r \Rightarrow s} (r_1)
\qquad\qquad
\frac{q \Rightarrow p \qquad q \Rightarrow \neg p}{q \Rightarrow \bot} (r_2)
$$

#### Run!

With the yaml in your hands (say it has name `logic.yaml` and it is located in the folder `/path/to/yaml/folder/`), run the following command:

```
sudo docker run -v \
/path/to/yaml/folder/:/input \
logicantsy ./ltsy rule-soundness-checker -f /input/logic.yaml
```

For example, if you create `logic.yaml` in inside the `logicantsy` folder, simply run from this same folder:
```
sudo docker run -v .:/input \
logicantsy ./ltsy rule-soundness-checker -f /input/logic.yaml
```

The program will output, for each rule, whether the rule is sound or not,
and, if not, it will provide countermodels (i.e., valuations that satisfy the premises and do not satisfy the conclusion).


