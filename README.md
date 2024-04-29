# Lean 4 tactics — overview for beginners

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

## Tactics for logical symbols

The following table shows usual responses to logical symbols based on their placement.

| Logical symbol                        | Appears in goal                         | Appears in hypothesis                                                                                                       |
|---------------------------------------|-----------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| <code>∀</code>&ensp; (for all)        | <code>intro *new_name*</code>           | <code>apply *expr*</code> or <code>specialize *name* *expr*</code>                                                 <tr></tr>|
| <code>∃</code>&ensp; (there exists)   | <code>use *expr*</code>                 | <code>obtain ⟨*new_name*, *new_name*⟩ := *expr*</code>                                                             <tr></tr>|
| <code>→</code>&ensp; (implies)        | <code>intro *new_name*</code>           | <code>apply *expr*</code> or <code>specialize *name* *expr*</code>                                                 <tr></tr>|
| <code>↔</code>&ensp; (if and only if) | <code>constructor</code>                | <code>rw [*expr*]</code> or <code>rw [←*expr*]</code>                                                             <tr></tr>|
| <code>∧</code>&ensp; (and)            | <code>constructor</code>                | <code>obtain ⟨*new_name*, *new_name*⟩ := *expr*</code>                                                             <tr></tr>|
| <code>∨</code>&ensp; (or)             | <code>left</code> or <code>right</code> | <code>cases *expr* with</code> <br><code>\| inl *new_name* => ...</code> <br><code>\| inr *new_name* => ...</code> <tr></tr>|
| <code>¬</code>&ensp; (not)            | <code>intro *new_name*</code>           | <code>apply *expr*</code> or <code>specialize *name* *expr*</code>                                                          |

## Generally useful tactics

In the left-hand column, the parts in parentheses are optional.
The effect of these parts in the right-hand column is also written in parentheses.

| Tactic                                                      | Effect                                                                                                                                                                                                                                      |
|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <code>exact *expr*</code>                                   | assert that the goal can be satisfied by <code>*expr*</code>                                                                                                                                                                       <tr></tr>|
| <code>refine</code>                                         | TODO                                                                                                                                                                                                                               <tr></tr>|
| <code>convert *expr*</code>                                 | prove the goal by transforming it to an existing fact <code>*expr*</code> and create goals for propositions used in the transformation that were not proved automatically                                                          <tr></tr>|
| <code>convert_to *proposition*</code>                       | transform the goal into the goal <code>*proposition*</code> and create additional goals for propositions used in the transformation that were not proved automatically                                                             <tr></tr>|
| <code>have *new_name* : *proposition*</code>                | introduce a name <code>*new_name*</code> asserting that <code>*proposition*</code> is true; at the same time, create and focus a goal for <code>*proposition*</code>                                                               <tr></tr>|
| <code>unfold *name*</code>&ensp;(<code>at *hyp*</code>)     | unfold the definition of <code>*name*</code> in the goal (or in the hypothesis <code>*hyp*</code>)                                                                                                                                 <tr></tr>|
| <code>rw [*expr*]</code>&ensp;(<code>at *hyp*</code>)       | in the goal (or in the hypothesis <code>*hyp*</code>), replace (all occurrences of) the left-hand side of the equality or equivalence <code>*expr*</code> by its right-hand side                                                   <tr></tr>|
| <code>rw [←*expr*]</code>&ensp;(<code>at *hyp*</code>)      | in the goal (or in the hypothesis <code>*hyp*</code>), replace (all occurrences of) the right-hand side of the equality or equivalence <code>*expr*</code> by its left-hand side                                                   <tr></tr>|
| <code>rw [*expr*, *expr*, ←*expr*, ←*expr*, *expr*]</code>  | perform several rewritings in a sequence (can again be used in the goal or in given hypothesis; any subset of <code>*expr*</code> can again be applied from right to left)                                                         <tr></tr>|
| <code>calc</code>                                           | start a proof by calculation (i.e., a sequence of propositions that, when combined together using transitivity, will prove the current goal)                                                                                       <tr></tr>|
| <code>gcongr</code>                                         | TODO                                                                                                                                                                                                                               <tr></tr>|
| <code>by_cases *new_name* : *proposition*</code>            | split the proof into two branches depending on whether <code>*proposition*</code> is true or false, using <code>*new_name*</code> as name for the respective hypothesis in both branches                                           <tr></tr>|
| <code>exfalso</code>                                        | apply the rule "False implies anything" a.k.a. "ex falso quodlibet" (replaces the current goal by <code>False</code>)                                                                                                              <tr></tr>|
| <code>by_contra *new_name*</code>                           | start a proof by contradiction, using <code>*new_name*</code> as name for the hypothesis that is the negation of the goal                                                                                                          <tr></tr>|
| <code>push_neg</code>&ensp;(<code>at *hyp*</code>)          | push negations in the goal (or in the hypothesis <code>*hyp*</code>),<br>e.g. change <code>¬ ∀ x, *proposition*</code> to <code>∃ x, ¬ *proposition*</code>                                                                        <tr></tr>|
| <code>linarith</code>                                       | prove the goal by a linear combination of hypotheses (includes arguments based on transitivity)                                                                                                                                    <tr></tr>|
| <code>ring</code>                                           | prove the goal by combining the axioms of a commutative (semi)ring                                                                                                                                                                 <tr></tr>|
| <code>simp</code>&ensp;(<code>at *hyp*</code>)              | simplify the goal (or the hypothesis <code>*hyp*</code>) by combining some standard equalities and equivalences                                                                                                                    <tr></tr>|
| <code>simp?</code>&ensp;(<code>at *hyp*</code>)             | show which equalities and equivalences would be used to simplify the goal (or the hypothesis <code>*hyp*</code>); give a list of expressions that can be used inside <code>simp only [...]</code>&ensp;(<code>at *hyp*</code>)     <tr></tr>|
| <code>exact?</code>                                         | search for a single existing lemma which closes the goal, also using local hypotheses                                                                                                                                              <tr></tr>|
| <code>apply?</code>                                         | search for lemmas whose conclusion matches the current goal; suggest those that may be used with <code>apply</code> or <code>refine</code>                                                                                         <tr></tr>|
| <code>rw?</code>&ensp;(<code>at *hyp*</code>)               | search for lemmas that can rewrite the current goal (or the hypothesis <code>*hyp*</code>), irrespective of their practicality                                                                                                     <tr></tr>|
| <code>aesop</code>                                          | TODO                                                                                                                                                                                                                                        |
