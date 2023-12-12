# Tàctiques Lean 4 per principiants

En les taules que segueixen, <code>*nom*</code> sempre fa referència a un nom que Lean ja coneix
mentre que <code>*nom_nou*</code> és un nom nou que podem escollir;
<code>*expr*</code> denota una expressió,
per exemple el nom d'un objecte en el context,
una expressió aritmètica que depèn d'aquests objectes,
una hipòtesi que tenim al context,
o un lema aplicat a qualsevol d'aquests;
<code>*proposició*</code> és una expressió de l'estil <code>Prop</code> (e.g. <code>0 < x</code>). 
Quan una d'aquestes paraules apareix dues vegades a la mateixa cel·la,
no vol dir que els noms o expressions hagin de ser les mateixes.

| Símbol lògic                        | Apareix a l'objectiu                        | Apareix en una hipòtesi                                                                                                           |
|---------------------------------------|-----------------------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| <code>∀</code>&ensp; (per tot)        | <code>intro *nom_nou*</code>           | <code>apply *expr*</code> o <code>specialize *nom* *expr*</code>                                                     <tr></tr>|
| <code>∃</code>&ensp; (existeix)   | <code>use *expr*</code>                 | <code>cases *expr* with</code> <br><code>  \| intro *nom_nou* *nom_nou* => ...</code>                                <tr></tr>|
| <code>→</code>&ensp; (implica)        | <code>intro *nom_nou*</code>           | <code>apply *expr*</code> o <code>specialize *nom* *expr*</code>                                                     <tr></tr>|
| <code>↔</code>&ensp; (si i només si) | <code>constructor</code>                | <code>rw [*expr*]</code> o <code>rw [←*expr*]</code>                                                                 <tr></tr>|
| <code>∧</code>&ensp; (i)            | <code>constructor</code>                | <code>cases *expr* with</code> <br><code>  \| intro *nom_nou* *nom_nou* => ...</code>                                <tr></tr>|
| <code>∨</code>&ensp; (o)             | <code>left</code> or <code>right</code> | <code>cases *expr* with</code> <br><code>  \| inl *nom_nou* => ...</code> <br><code>  \| inr *nom_nou* => ...</code> <tr></tr>|
| <code>¬</code>&ensp; (no)            | <code>intro *nom_nou*</code>           | <code>apply *expr*</code> o <code>specialize *nom* *expr*</code>                                                              |

A l'esquerra de la taula següent les parts entre parèntesis són opcionals.
L'efecte d'aquestes parts també està escrit entre parèntesis.

| Tàctica                                                      | Efecte                                                                                                                                                                                                                                      |
|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <code>exact *expr*</code>                                   | demostra que l'objectiu es pot provar amb <code>*expr*</code>                                                                                                                                                                       <tr></tr>|
| <code>refine</code>                                         | Igual que <code>exact *expr*</code>, però podem deixar <code>?_</code> en algunes dades i crearà nous objectius                                                                                                                                                                                                                               <tr></tr>|
| <code>convert *expr*</code>                                 | demostra l'objectiu transformant-lo en un fet ja existent <code>*expr*</code> i crea nous objectius per aquelles proposicions utilitzades en la transformació que no s'hagin demostrat automàticament <tr></tr>|
| <code>convert_to *proposition*</code>                       | transforma l'objectiu en el nou objectiu <code>*proposició*</code> i crea nous objectius per aquells proposicions utilitzades en la transformació que no s'hagin demostrat automàticament                                                             <tr></tr>|
| <code>have *nom_nou* : *proposition*</code>                | introduceis un nom <code>*nom_nou*</code> enunciant que la <code>*proposició*</code> és certa; simultàniament, crea un nou objectiu per demostrar la <code>*proposició*</code>                                                               <tr></tr>|
| <code>unfold *nom*</code>&ensp;(<code>at *hyp*</code>)     | reescriu la definició de <code>*nom*</code> a l'objectiu (o a la hipòtesi <code>*hyp*</code>)                                                                                                                                 <tr></tr>|
| <code>rw [*expr*]</code>&ensp;(<code>at *hyp*</code>)       | a l'objectiu (o a la hipòtesi <code>*hyp*</code>), canvia (totes es instàncies de) la part esquerra de la igualtat o equivalència <code>*expr*</code> per la part dreta                                                   <tr></tr>|
| <code>rw [←*expr*]</code>&ensp;(<code>at *hyp*</code>)      | a l'objectiu (o a la hipòtesi <code>*hyp*</code>), canvia (totes es instàncies de) la part dreta de la igualtat o equivalència <code>*expr*</code> per la part esquerra                                                  <tr></tr>|
| <code>rw [*expr*, *expr*, ←*expr*, ←*expr*, *expr*]</code>  | fer diverses reescriptures l'una darrera l'altra (es pot fer servir a l'objectiu o a una hipòtesi donada; es pot alternar canvis esquerra-dreta amb dreta-esquerra)                                                         <tr></tr>|
| <code>calc</code>                                           | comença una demostració per càlcul (i.e. una successió de proposicions que, al combinar-se fent servir transitivitat, demostraran l'objectiu)                                                                                       <tr></tr>|
| <code>gcongr</code>                                         | TODO                                                                                                                                                                                                                               <tr></tr>|
| <code>by_cases *nom_nou* : *proposició*</code>            | trenca la demostració en dues branques segons <code>*proposició*</code> sigui cert o fals, fent servir <code>*nom_nou*</code> com el nom de la hipòtesi respecriva en cada branca                                          <tr></tr>|
| <code>exfalso</code>                                        | aplica la regla "Fals implica qualsevol cosa", també coneguda com "ex falso quodlibet" (canvia l'objectiu a  <code>False</code>)                                                                                                              <tr></tr>|
| <code>by_contra *nom_nou*</code>                           | comença una demostració per reducció a l'absurd, fent servir <code>*nom_nou*</code> per la hipòtesi que és la negació de l'objectiu                                                     <tr></tr>|
| <code>push_neg</code>&ensp;(<code>at *hyp*</code>)          | simplifica les negacions a l'objectiu (o a la hipòtesi <code>*hyp*</code>),<br>e.g. canvia <code>¬ ∀ x, *proposició*</code> to <code>∃ x, ¬ *proposició*</code>                                                                        <tr></tr>|
| <code>linarith</code>                                       | demostra l'objectiu fent servir una combinació lineal de les hipòtesis (inclosos arguments basats en la transitivitat)                                                                                                                                    <tr></tr>|
| <code>ring</code>                                           | demostra l'objectiu combinant els axiomes d'un (semi)anell commutatiu                                                                                                                                                                 <tr></tr>|
| <code>simp</code>&ensp;(<code>at *hyp*</code>)              | simplifica l'objectiu (o la hipòtesi <code>*hyp*</code>) combinant algunes igualtats i equivalències ja apreses                                                                                                                    <tr></tr>|
| <code>simp?</code>&ensp;(<code>at *hyp*</code>)             | mostra quines igualtats i equivalències es farien servir per simplificar l'objectiu (o la hipòtesi <code>*hyp*</code>); dona una llista d'expressions que poden utilitzar-se dins de <code>simp only [...]</code>&ensp;(<code>at *hyp*</code>)     <tr></tr>|
| <code>exact?</code>                                         | busca un lema que demostri l'objectiu fent servir hipòtesis del context                                                                                                                                              <tr></tr>|
| <code>apply?</code>                                         | busca lemes la conclusió dels quals coincideixi amb l'objectiu; suggereix aquells que es poden fer servir amb  <code>apply</code> o <code>refine</code>                                                                                         <tr></tr>|
| <code>rw?</code>                                            | TODO                                                                                                                                                                                                                               <tr></tr>|
| <code>aesop</code>                                          | TODO                                                                                                                                                                                                                                        |