# Lean 4 — přehled taktik pro začátečníky

V následujících tabulkách vždy <code>*jmeno*</code> označuje jméno, které už Lean zná,
zatímco <code>*pojmenovani*</code> je nové jméno (právě poskytnuté);
<code>*term*</code> je libovolný výraz,
například jméno známého termu,
funkce aplikovaná na term,
aritmetický výraz,
předpoklad z lokálního kontextu,
nebo nějaké lemma aplikované na cokoliv z toho;
<code>*tvrzeni*</code> je term typu <code>Prop</code> (například <code>0 < x</code>). 
Když se některé z těchto slov objeví v jedné buňce vícekrát,
jednotlivé výskyty označují odlišná jména nebo výrazy.

## Taktiky pro logické symboly

Následující tabulka ukazuje, jak typicky reagujeme na který logický symbol v závislosti
na jeho umístění (v čele cíle nebo v čele předpokladu).

| Logický symbol                          | V cíli                                    | V předpokladu                                                                                                                     |
|-----------------------------------------|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------|
| <code>∀</code>&ensp; (pro každé)        | <code>intro *pojmenovani*</code>          | <code>apply *term*</code> nebo <code>specialize *jmeno* *term*</code>                                                    <tr></tr>|
| <code>∃</code>&ensp; (existuje)         | <code>use *term*</code>                   | <code>obtain ⟨*pojmenovani*, *pojmenovani*⟩ := *term*</code>                                                             <tr></tr>|
| <code>→</code>&ensp; (implikuje)        | <code>intro *pojmenovani*</code>          | <code>apply *term*</code> nebo <code>specialize *jmeno* *term*</code>                                                    <tr></tr>|
| <code>↔</code>&ensp; (právě tehdy když) | <code>constructor</code>                  | <code>rw [*term*]</code> nebo <code>rw [←*term*]</code>                                                                  <tr></tr>|
| <code>∧</code>&ensp; (AND)              | <code>constructor</code>                  | <code>obtain ⟨*pojmenovani*, *pojmenovani*⟩ := *term*</code>                                                             <tr></tr>|
| <code>∨</code>&ensp; (OR)               | <code>left</code> nebo <code>right</code> | <code>cases *term* with</code> <br><code>\| inl *pojmenovani* => ...</code> <br><code>\| inr *pojmenovani* => ...</code> <tr></tr>|
| <code>¬</code>&ensp; (NOT)              | <code>intro *pojmenovani*</code>          | <code>apply *term*</code> nebo <code>specialize *jmeno* *term*</code>                                                             |

Závorky v levém sloupci následující tabulky označují volitelné části.
Účinek těchto částí je v pravém sloupci také napsán v závorkách.

## Obecně užitečné taktiky

| Taktika                                                     | Co dělá                                                                                                                                                                                                                                     |
|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <code>exact *term*</code>                                   | cíl je splněn pomocí <code>*term*</code>                                                                                                                                                                                           <tr></tr>|
| <code>convert *term*</code>                                 | dokaž cíl tím, že ho převedeš na již známý fakt <code>*term*</code>, a vytvoř pomocné cíle pro ty transformace použité k převodu, které nebyly dokázány automaticky                                                                <tr></tr>|
| <code>convert_to *tvrzeni*</code>                           | dokaž cíl tím, že ho převedeš na jiný cíl <code>*tvrzeni*</code>, a vytvoř dodatečné cíle pro ty transformace použité k převodu, které nebyly dokázány automaticky                                                                 <tr></tr>|
| <code>have *pojmenovani* : *tvrzeni*</code>                 | přidej do kontextu předpoklad <code>*pojmenovani*</code>, který tvrdí <code>*tvrzeni*</code>, a zároveň vytvoř a zaměř nový cíl dokázat <code>*tvrzeni*</code>                                                                     <tr></tr>|
| <code>unfold *jmeno*</code>&ensp;(<code>at *hyp*</code>)    | rozbal definici <code>*jmeno*</code> v cíli (nebo v předpokladu <code>*hyp*</code>)                                                                                                                                                <tr></tr>|
| <code>rw [*term*]</code>&ensp;(<code>at *hyp*</code>)       | v cíli (nebo v předpokladu <code>*hyp*</code>) nahraď všechny výskyty levé strany rovnosti nebo ekvivalence <code>*term*</code> za její pravou stranu                                                                              <tr></tr>|
| <code>rw [←*term*]</code>&ensp;(<code>at *hyp*</code>)      | v cíli (nebo v předpokladu <code>*hyp*</code>) nahraď všechny výskyty pravé strany rovnosti nebo ekvivalence <code>*term*</code> za její levou stranu                                                                              <tr></tr>|
| <code>rw [*term*, *term*, ←*term*, ←*term*, *term*]</code>  | proveď několik přepisů v řadě za sebou (opět může být použito v cíli i v předpokladu; libovolná podmnožina <code>*term*</code> může být provedena zprava doleva)                                                                   <tr></tr>|
| <code>by_cases *pojmenovani* : *tvrzeni*</code>             | rozlož důkaz na dvě větve podle toho, jestli <code>*tvrzeni*</code> je splněn nebo ne, kde <code>*pojmenovani*</code> označuje kýženou hypotézu v obou větvích                                                                     <tr></tr>|
| <code>exfalso</code>                                        | použij pravidlo "ze sporu plyne cokoliv" neboli "ex falso quodlibet" (nahradí současný cíl za <code>False</code>)                                                                                                                  <tr></tr>|
| <code>by_contra *pojmenovani*</code>                        | zahaj důkaz sporem, kde <code>*pojmenovani*</code> bude jméno pro předpoklad, který je negací cíle                                                                                                                                 <tr></tr>|
| <code>push_neg</code>&ensp;(<code>at *hyp*</code>)          | posuň negace směrem „dovnitř“ v cíli (nebo v předpokladu <code>*hyp*</code>),<br>například změň <code>¬ ∀ x, *tvrzeni*</code> na <code>∃ x, ¬ *tvrzeni*</code>                                                                     <tr></tr>|
| <code>linarith</code>                                       | dokaž cíl pomocí lineární kombinace předpokladů (zahrnuje argumenty z tranzitivity (ne)rovností)                                                                                                                                   <tr></tr>|
| <code>ring</code>                                           | dokaž cíl pomocí vlastností sčítání, odčítání, násobení a mocnin                                                                                                                                                                   <tr></tr>|
| <code>simp</code>&ensp;(<code>at *hyp*</code>)              | zjednoduš cíl (nebo předpoklad <code>*hyp*</code>) pomocí nějaké kombinace známých rovností a ekvivalencí                                                                                                                          <tr></tr>|
| <code>simp?</code>&ensp;(<code>at *hyp*</code>)             | ukaž, které rovnosti a ekvivalence by mohly býž použity ke zjednodušení cíle (nebo předpokladu <code>*hyp*</code>); dej seznam výrazů, které mohou být použity uvnitř <code>simp only [...]</code>&ensp;(<code>at *hyp*</code>)    <tr></tr>|
| <code>library_search</code>                                 | najdi v knihovně (navíc s využitím lokálního kontextu) lemma, které uzavře cíl                                                                                                                                                     <tr></tr>|
