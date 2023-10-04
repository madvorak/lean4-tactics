# Overview of tactics in Lean 4 for beginners

In the following table, <code>*name*</code> always refers to a name already known to Lean
while <code>*new_name*</code> refers to a new name provided by the user;
<code>*expr*</code> designates an expression,
for example the name of an object in the context,
an arithmetic expression that is a function of such objects,
a hypothesis in the context,
or a lemma applied to any of these.
When one of these words appears twice in the same line,
the appearances do not designate the same name or
the same expression.

| Logical symbol   | Appears in goal                         | Appears in hypothesis                                                                                |
|------------------|-----------------------------------------|------------------------------------------------------------------------------------------------------|
| (for all)        | <code>intro *new_name*</code>           | <code>apply *expr*</code> or <code>specialize *name* *expr*</code>                                   |
| (there exists)   |                                         | <code>cases *expr* with</code> <br><code>  \| inl a => ...</code> <br><code>  \| inr b => ...</code> |
| (implies)        |                                         |                                                                                                      |
| (if and only if) |                                         |                                                                                                      |
| (and)            |                                         |                                                                                                      |
| (or)             | <code>left</code> or <code>right</code> |                                                                                                      |
| (not)            |                                         |                                                                                                      |
