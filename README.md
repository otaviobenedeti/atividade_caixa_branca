# Teste de Caixa Branca — Sistema de Pedidos

## 1. Contextualização sobre Teste de Caixa Branca

O Teste de Caixa Branca é uma técnica de teste de software que analisa a estrutura interna do código, verificando as condições, decisões, caminhos de execução, operadores e alterações das variáveis.

Neste trabalho, foi analisado um sistema de pedidos desenvolvido com **HTML, CSS e JavaScript**, responsável por calcular valores de produtos, descontos, frete e total da compra.

O objetivo foi identificar erros na lógica do programa, criar casos de teste para cada erro e verificar o comportamento do sistema antes e depois das correções.

---

## 2. Erros Identificados

Durante a análise do código foram identificados **6 erros principais** relacionados às estruturas de decisão e aos cálculos realizados pelo sistema.

### Erro 1 — Cupom SENAI20

**Problema:**
O desconto de 20% era aplicado quando o subtotal era maior ou igual a R$ 500,00.

**Comportamento correto:**
O cupom deve funcionar somente quando o subtotal for maior ou igual a R$ 1.000,00.

**Correção:**

```javascript
if (codigo === "SENAI20" && subtotal >= 1000) {
    return subtotal * 0.20;
}
```

---

### Erro 2 — Frete grátis

**Problema:**
O frete grátis era aplicado somente quando o subtotal era maior que R$ 500,00.

**Comportamento correto:**
Compras com subtotal exatamente igual a R$ 500,00 também devem possuir frete grátis.

**Correção:**

```javascript
if (subtotal >= 500) {
    return 0;
}
```

---

### Erro 3 — Validação da quantidade

**Problema:**
O sistema permitia uma quantidade igual a zero.

**Comportamento correto:**
A quantidade deve ser considerada inválida quando for menor ou igual a zero.

**Correção:**

```javascript
if (qtd <= 0) {
    resultado.innerHTML = "<p>Quantidade inválida.</p>";
    return;
}
```

---

### Erro 4 — Validação do estoque

**Problema:**
O sistema rejeitava a compra quando a quantidade solicitada era igual ao estoque disponível.

**Comportamento correto:**
A compra deve ser permitida quando a quantidade for exatamente igual ao estoque. Somente quantidades maiores que o estoque devem ser rejeitadas.

**Correção:**

```javascript
if (qtd > estoque[produtoSelecionado]) {
    resultado.innerHTML = "<p>Quantidade indisponível em estoque.</p>";
    return;
}
```

---

### Erro 5 — Desconto para mais de 5 unidades

**Problema:**
O desconto de 5% era calculado multiplicando o valor total por 0,95.

**Comportamento correto:**
O desconto de 5% deve ser calculado sobre o subtotal do pedido.

**Correção:**

```javascript
if (qtd > 5) {
    total = total - subtotal * 0.05;
}
```

---

### Erro 6 — Desconto para pedidos de alto valor

**Problema:**
O desconto de 5% para pedidos de alto valor era aplicado somente quando o total era maior que R$ 3.000,00.

**Comportamento correto:**
O desconto também deve ser aplicado quando o total for exatamente R$ 3.000,00.

**Correção:**

```javascript
if (total >= 3000) {
    total = total * 0.95;
}
```

---

### Resumo dos erros

| Erro | Problema                                         | Correção                                      |
| ---- | ------------------------------------------------ | --------------------------------------------- |
| 1    | Cupom SENAI20 a partir de R$ 500                 | Cupom a partir de R$ 1.000                    |
| 2    | Frete grátis somente acima de R$ 500             | Frete grátis a partir de R$ 500               |
| 3    | Quantidade zero era aceita                       | Quantidade menor ou igual a zero é inválida   |
| 4    | Quantidade igual ao estoque era rejeitada        | Rejeitar somente quando ultrapassar o estoque |
| 5    | Desconto de 5% calculado incorretamente          | Desconto calculado sobre o subtotal           |
| 6    | Desconto de alto valor somente acima de R$ 3.000 | Aplicar também em R$ 3.000                    |

---

## 2. Análise das Estruturas de Decisão

Durante a análise foram observadas diversas estruturas de decisão no código.

### Estrutura `if`

O sistema utiliza vários comandos `if` para verificar condições.

Exemplo:

```javascript
if (qtd <= 0) {
    resultado.innerHTML = "<p>Quantidade inválida.</p>";
    return;
}
```

Essa condição verifica se a quantidade informada pelo usuário é válida.

Outro exemplo:

```javascript
if (total >= 3000) {
    total = total * 0.95;
}
```

Nesse caso, o sistema verifica se o valor do pedido atingiu o limite necessário para receber o desconto.

### Estrutura `if / else if`

Também existe uma sequência de decisões para definir a mensagem final:

```javascript
if (total <= 0) {
    mensagem = "Valor do pedido inválido.";
} else if (total >= 3000) {
    mensagem = "Pedido de alto valor.";
}
```

O programa analisa as condições em ordem e executa apenas o bloco correspondente.

### Operadores utilizados

Foram analisados principalmente os seguintes operadores:

* `<=` — menor ou igual
* `>=` — maior ou igual
* `>` — maior que
* `===` — igualdade
* `&&` — operador lógico E
* `-` — subtração
* `*` — multiplicação

A alteração desses operadores pode modificar completamente o caminho de execução do programa.

---

## 3. Fluxograma do Exemplo

O fluxo principal do sistema pode ser representado da seguinte forma:

```text
        INÍCIO
           |
           v
   Selecionar produto
           |
           v
   Informar quantidade
           |
           v
    Quantidade <= 0?
       /         \
     SIM          NÃO
      |             |
      v             v
  Quantidade    Verificar
   inválida       estoque
                    |
                    v
             Quantidade > estoque?
                /          \
              SIM           NÃO
               |              |
               v              v
          Estoque        Calcular subtotal
         insuficiente          |
                               v
                       Calcular desconto
                               |
                               v
                        Calcular frete
                               |
                               v
                     Aplicar desconto
                      de quantidade
                               |
                               v
                       Total >= 3000?
                         /        \
                       SIM         NÃO
                        |            |
                        v            |
                 Aplicar 5%          |
                    |               |
                    +-------+-------+
                            |
                            v
                  Exibir resultado
                            |
                            v
                           FIM
```

Esse fluxo permite visualizar os principais caminhos de execução presentes no sistema.

---

## 4. Casos de Teste

Foram criados **6 casos de teste**, sendo um para cada erro identificado.

### CT01 — Cupom SENAI20

**Objetivo:** verificar o limite de aplicação do cupom de 20%.

**Dados de entrada:**

* Produto: Notebook
* Quantidade: 1
* Subtotal: R$ 3.000,00
* Cupom: `SENAI20`

**Resultado esperado:**
O desconto de 20% deve ser aplicado, pois o subtotal é maior que R$ 1.000,00.

**Técnica utilizada:** Valor limite.

---

### CT02 — Frete grátis

**Objetivo:** verificar o limite do frete grátis.

**Dados de entrada:**

* Subtotal: R$ 500,00
* Frete: normal

**Resultado esperado:**
O frete deve ser R$ 0,00.

**Técnica utilizada:** Valor limite.

---

### CT03 — Quantidade igual a zero

**Objetivo:** verificar a validação da quantidade.

**Dados de entrada:**

* Quantidade: 0

**Resultado esperado:**
O sistema deve apresentar a mensagem:

```text
Quantidade inválida.
```

**Técnica utilizada:** Valor limite.

---

### CT04 — Quantidade igual ao estoque

**Objetivo:** verificar se o sistema permite comprar exatamente a quantidade disponível.

**Dados de entrada:**

* Produto: Notebook
* Estoque: 5 unidades
* Quantidade: 5 unidades

**Resultado esperado:**
O pedido deve ser aceito, pois a quantidade solicitada é igual ao estoque disponível.

**Técnica utilizada:** Valor limite.

---

### CT05 — Mais de 5 unidades

**Objetivo:** verificar o cálculo do desconto para compras com mais de 5 unidades.

**Dados de entrada:**

* Produto: Mouse
* Quantidade: 6
* Preço unitário: R$ 80,00
* Subtotal: R$ 480,00

**Resultado esperado:**
O desconto de 5% deve ser calculado sobre o subtotal:

```text
R$ 480,00 × 5% = R$ 24,00
```

**Técnica utilizada:** Análise de condição e rastreamento de variável.

---

### CT06 — Pedido de alto valor

**Objetivo:** verificar o limite do desconto para pedidos de alto valor.

**Dados de entrada:**

* Total antes do desconto: R$ 3.000,00

**Resultado esperado:**
O desconto de 5% deve ser aplicado, pois o valor é exatamente R$ 3.000,00.

**Técnica utilizada:** Valor limite.

---

## 5. Resultados dos Testes

Após a correção do código, os casos de teste devem apresentar os resultados esperados.

| Caso | Situação testada            | Resultado esperado               | Resultado |
| ---- | --------------------------- | -------------------------------- | --------- |
| CT01 | Cupom SENAI20               | Aplicar 20% a partir de R$ 1.000 | Corrigido |
| CT02 | Frete grátis                | Frete grátis em R$ 500           | Corrigido |
| CT03 | Quantidade 0                | Exibir quantidade inválida       | Corrigido |
| CT04 | Quantidade igual ao estoque | Permitir compra                  | Corrigido |
| CT05 | Mais de 5 unidades          | Aplicar 5% sobre subtotal        | Corrigido |
| CT06 | Total de R$ 3.000           | Aplicar 5% de desconto           | Corrigido |

### Evidências

As capturas de tela dos testes podem ser adicionadas nesta seção:

```text
/imagens/teste01.png
/imagens/teste02.png
/imagens/teste03.png
/imagens/teste04.png
/imagens/teste05.png
/imagens/teste06.png
```

Exemplo:

```markdown
![Teste 01](imagens/teste01.png)
```

---

## 6. Análise dos Resultados

A análise dos testes mostrou que os erros estavam principalmente relacionados aos **valores limite das condições**.

Os operadores `>`, `>=`, `<=` e `<` possuem grande importância no comportamento do sistema. Uma pequena alteração nesses operadores pode fazer com que determinados valores sejam aceitos ou rejeitados incorretamente.

Os testes também permitiram verificar o caminho de execução das condições e acompanhar as alterações das variáveis durante o processamento do pedido.

Após as correções, os casos de teste passaram a seguir o comportamento esperado para cada situação analisada.

---

## 7. Conclusão

O Teste de Caixa Branca permitiu analisar detalhadamente a estrutura interna do sistema de pedidos.

Foram identificados **6 erros**, principalmente relacionados a condições, limites, estoque e cálculos de descontos.

A aplicação das técnicas de cobertura de comandos, análise de decisões, análise de caminhos, valores limite e rastreamento de variáveis ajudou a encontrar e corrigir os problemas.

Com os testes realizados após as correções, foi possível verificar que o sistema passou a apresentar o comportamento esperado nos casos analisados.
