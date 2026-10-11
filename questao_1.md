# Resolução do Trabalho: Raciocínio Probabilístico

## (a) Desenho da Rede Bayesiana de Causalidade

As variáveis do problema são: `Str` (Condição da rua), `Flw` (Desgaste do volante), `R` (Deslizamento), `V` (Tensão), `B` (Lâmpada), `K` (Cabo) e `Li` (Luz). A rede é construída traduzindo as relações de causa e efeito físicas e respeitando as equações de independência dadas:

* **Raízes (sem pais):** `Str`, `Flw`, `B` e `K` são independentes aos pares, logo não têm nós ascendentes.
* **Causas do Deslizamento (`R`):** O dínamo desliza devido à condição da rua (`Str`) e ao desgaste do volante (`Flw`). Arestas: `Str` -> `R` e `Flw` -> `R`.
* **Causas da Tensão (`V`):** A geração de tensão depende se o dínamo desliza ou não. As equações P(V|R,Str)=P(V|R) e P(V|R,Flw)=P(V|R) confirmam que `V` é independente de `Str` e `Flw` dado `R`. Aresta: `R` -> `V`.
* **Causas da Luz (`Li`):** Para a luz ligar, é preciso haver tensão (`V`), a lâmpada estar em bom estado (`B`) e o cabo estar ok (`K`). Arestas: `V` -> `Li`, `B` -> `Li`, `K` -> `Li`.

```
Str         Flw           B           K  
  \         /             |          /  
   \       /              |         /  
    v     v               v        v  
       R                     Li  
       |                     ^  
       v                     |  
       V --------------------+
```

## (b) CPTs (Tabelas de Probabilidade Condicional) em Falta

O enunciado fornece a tabela para as probabilidades de `Li` dadas as variáveis `V`, `B` e `K`. Para completar a rede bayesiana, faltam as seguintes CPTs:

1. **Probabilidades a priori:** P(Str), P(Flw), P(B) e P(K).
2. **Probabilidades condicionais:** P(R | Str, Flw) e P(V | R).

## (c) Inserção de Valores Plausíveis

Valores lógicos atribuídos livremente para permitir os cálculos da alínea (e):

**Probabilidades a Priori:**

* P(Flw = t) = 0.1
* P(Flw = f) = 0.9

**CPT de P(R | Str, Flw) (Probabilidade do dínamo deslizar):**

* P(R = t | Str = snow_covered, Flw = t) = 0.8
* P(R = t | Str = snow_covered, Flw = f) = 0.4

**CPT de P(V | R) (Probabilidade de gerar tensão):**

* P(V = t | R = t) = 0.2 (Se desliza, gera pouca tensão)
* P(V = t | R = f) = 0.95 (Se não desliza, quase sempre gera tensão)

## (d) Prova de Inexistência da Aresta (Str, Li)

Não existe uma aresta causal direta entre a condição da rua (`Str`) e a luz (`Li`). A equação P(Li|V,R)=P(Li|V) garante que qualquer impacto de `Str` e `R` na luz é totalmente absorvido assim que o estado de `V` é conhecido. Por d-separação, o caminho causal está bloqueado no nó `V`, demonstrando que a aresta direta `(Str, Li)` não existe na rede.

## (e) Cálculo de P(V | Str = snow_covered)

**Passo 1: Calcular P(R=t | Str=snow_covered)**  

Aplicando a Lei da Probabilidade Total sobre a variável `Flw`:  

P(R=t | snow) = [ P(R=t | snow, Flw=t) * P(Flw=t) ] + [ P(R=t | snow, Flw=f) * P(Flw=f) ]  
P(R=t | snow) = (0.8 * 0.1) + (0.4 * 0.9)  
P(R=t | snow) = 0.08 + 0.36 = 0.44  

Logo, a probabilidade complementar é P(R=f | snow) = 1 - 0.44 = 0.56.

**Passo 2: Calcular P(V=t | Str=snow_covered)**  

Aplicando novamente a Lei da Probabilidade Total, agora sobre a variável `R`:  

P(V=t | snow) = [ P(V=t | R=t) * P(R=t | snow) ] + [ P(V=t | R=f) * P(R=f | snow) ]  
P(V=t | snow) = (0.2 * 0.44) + (0.95 * 0.56)  
P(V=t | snow) = 0.088 + 0.532 = 0.62  

**Conclusão:** A probabilidade de o dínamo mostrar tensão quando a rua está coberta de neve é de **62%**.
