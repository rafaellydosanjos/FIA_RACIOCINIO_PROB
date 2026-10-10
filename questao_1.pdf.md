# Resolução do Trabalho: Raciocínio Probabilístico

## (a) Desenho da rede

As variáveis são Str, Flw, R, V, B, K e Li. Str, Flw, B e K são independentes entre si, por isso ficam como raízes, sem pais.

O deslizamento R depende da condição da rua e do desgaste do volante, logo Str → R e Flw → R. A tensão V depende só de R: pelas igualdades P(V | R, Str) = P(V | R) e P(V | R, Flw) = P(V | R), sabendo R, nem Str nem Flw acrescentam informação sobre V. Fica R → V. A luz precisa de tensão, lâmpada e cabo, então V → Li, B → Li e K → Li.

Arestas: Str → R, Flw → R, R → V, V → Li, B → Li, K → Li.

## (b) CPTs em falta

A tabela de Li dados V, B e K já vem no enunciado. Faltam as probabilidades a priori P(Str), P(Flw), P(B) e P(K), e as condicionais P(R | Str, Flw) e P(V | R).

## (c) Valores plausíveis

Para o cálculo da alínea (e) só preciso de parte das tabelas, por isso defini apenas essas entradas:

- P(Flw = t) = 0,1 e P(Flw = f) = 0,9
- P(R = t | Str = snow_covered, Flw = t) = 0,8
- P(R = t | Str = snow_covered, Flw = f) = 0,4
- P(V = t | R = t) = 0,2, porque se o dínamo desliza gera pouca tensão
- P(V = t | R = f) = 0,95, porque se não desliza quase sempre gera tensão

## (d) Porque não há aresta (Str, Li)

Na rede, o único caminho de Str até Li passa por R e depois por V (Str → R → V → Li). A igualdade P(Li | V, R) = P(Li | V) mostra que, uma vez conhecido V, o valor de R já não altera a luz, e o mesmo vale para Str, que só chega a Li através de R. Ou seja, V bloqueia o caminho (d-separação), e não é preciso uma ligação direta entre Str e Li.

## (e) Cálculo de P(V | Str = snow_covered)

Primeiro obtenho P(R = t | snow) somando sobre Flw:

```
P(R = t | snow) = P(R = t | snow, Flw = t) · P(Flw = t) + P(R = t | snow, Flw = f) · P(Flw = f)
                = 0,8 · 0,1 + 0,4 · 0,9
                = 0,08 + 0,36 = 0,44
```

Daqui, P(R = f | snow) = 1 − 0,44 = 0,56.

Agora somo sobre R para chegar a V:

```
P(V = t | snow) = P(V = t | R = t) · P(R = t | snow) + P(V = t | R = f) · P(R = f | snow)
                = 0,2 · 0,44 + 0,95 · 0,56
                = 0,088 + 0,532 = 0,62
```

Com a rua coberta de neve, a probabilidade de o dínamo mostrar tensão é 0,62, ou seja, 62%.
