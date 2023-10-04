# Overview of tactics in Lean 4 for beginners

In the following tables, <code>*name*</code> always refers to a name already known to Lean
while <code>*new_name*</code> is a new name provided by the user;
<code>*expr*</code> means an expression,
for example the name of an object in the context,
an arithmetic expression that is a function of such objects,
a hypothesis in the context,
or a lemma applied to any of these;
<code>*proposition*</code> is an expression of the type <code>Prop</code> (e.g. <code>0 < x</code>). 
When one of these words appears twice in the same cell,
the appearances do not designate the same name or the same expression.

| Logical symbol                        | Appears in goal                         | Appears in hypothesis                                                                                                  |
|---------------------------------------|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| <code>∀</code>&ensp; (for all)        | <code>intro *new_name*</code>           | <code>apply *expr*</code> or <code>specialize *name* *expr*</code>                                                     |
| <code>∃</code>&ensp; (there exists)   | <code>use *expr*</code>                 | <code>cases *expr* with</code> <br><code>  \| intro *new_name* *new_name* => ...</code>                                |
| <code>→</code>&ensp; (implies)        | <code>intro *new_name*</code>           | <code>apply *expr*</code> or <code>specialize *name* *expr*</code>                                                     |
| <code>↔</code>&ensp; (if and only if) | <code>constructor</code>                | <code>rw [*expr*]</code> or <code>rw [← *expr*]</code>                                                                 |
| <code>∧</code>&ensp; (and)            | <code>constructor</code>                | <code>cases *expr* with</code> <br><code>  \| intro *new_name* *new_name* => ...</code>                                |
| <code>∨</code>&ensp; (or)             | <code>left</code> or <code>right</code> | <code>cases *expr* with</code> <br><code>  \| inl *new_name* => ...</code> <br><code>  \| inr *new_name* => ...</code> |
| <code>¬</code>&ensp; (not)            | <code>intro *new_name*</code>           | <code>apply *expr*</code> or <code>specialize *name* *expr*</code>                                                     |

In the left-hand column of the following table, the parts in brackets are optional.
The effect of these parts is also in brackets in the right-hand column.

| Tactic                                                      | Effect                                                                                                                                                                                                                             |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <code>exact *expr*</code>                                   | assert that the goal can be satisfied by <code>*expr*</code>                                                                                                                                                                       |
| <code>refine</code>                                         | TODO                                                                                                                                                                                                                               |
| <code>convert *expr*</code>                                 | prove the goal by transforming it to an existing fact <code>*expr*</code> and create goals for propositions used in the transformation that were not proved automatically                                                          |
| <code>convert_to *proposition*</code>                       | transform the goal into the goal <code>*proposition*</code> and create additional goals for propositions used in the transformation that were not proved automatically                                                             |
| <code>have *new_name* : *proposition*</code>                | introduce a name <code>*new_name*</code> asserting that <code>*proposition*</code> is true; at the same time, create and focus a goal for <code>*proposition*</code>                                                               |
| <code>unfold *name*</code>&ensp;(<code>at *hyp*</code>)     | unfold the definition of <code>*name*</code> in the goal (or in the hypothesis <code>*hyp*</code>)                                                                                                                                 |
| <code>rw [*expr*]</code>&ensp;(<code>at *hyp*</code>)       | in the goal (or in the hypothesis <code>*hyp*</code>), replace (all occurrences of) the left-hand side of the equality or equivalence <code>*expr*</code> by its right-hand side (can include more arguments and/or more targets)  |
| <code>rw [← *expr*]</code>&ensp;(<code>at *hyp*</code>)     | in the goal (or in the hypothesis <code>*hyp*</code>), replace (all occurrences of) the right-hand side of the equality or equivalence <code>*expr*</code> by its left-hand side (can include more arguments and/or more targets)  |
| <code>calc</code>                                           | start a proof by calculation (i.e., a sequence of propositions that, when combined together using transitivity, prove the current goal                                                                                             |
| <code>gcongr</code>                                         | TODO                                                                                                                                                                                                                               |
| <code>by_cases *new_name* : *proposition*</code>            | split the proof into two branches depending on whether <code>*proposition*</code> is true or false, using <code>*new_name*</code> as name for the respective hypothesis in both branches                                           |
| <code>exfalso</code>                                        | apply the rule "False implies anything" a.k.a. "ex falso quodlibet" (replaces the current goal by <code>False</code>)                                                                                                              |
| <code>by_contra *new_name*</code>                           | start a proof by contradiction, using <code>*new_name*</code> as name for the hypothesis that is the negation of the goal                                                                                                          |
| <code>push_neg</code>&ensp;(<code>at *hyp*</code>)          | push negations in the goal (or in the hypothesis <code>*hyp*</code>),<br>e.g. change <code>¬ ∀ x, *proposition*</code> to <code>∃ x, ¬ *proposition*</code>                                                                        |
| <code>linarith</code>                                       | prove the goal by a linear combination of hypotheses (includes arguments based on transitivity)                                                                                                                                    |
| <code>ring</code>                                           | prove the goal by combining the axioms of a commutative (semi)ring                                                                                                                                                                 |
| <code>simp</code>&ensp;(<code>at *hyp*</code>)              | simplify the goal (or the hypothesis <code>*hyp*</code>) by combining some standard equalities and equivalences                                                                                                                    |
| <code>exact?</code>                                         | search for a single existing lemma which closes the goal, also using local hypotheses                                                                                                                                              |
| <code>apply?</code>                                         | search for lemmas whose conclusion matches the current goal; suggest those that may be used with <code>apply</code> or <code>refine</code> in the current context                                                                  |
| <code>rw?</code>                                            | TODO                                                                                                                                                                                                                               |
| <code>aesop</code>                                          | TODO                                                                                                                                                                                                                               |
