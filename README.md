# Organização da Análise

Este notebook apresenta o desenvolvimento de um modelo de regressão linear para estimar a quantidade de matéria verde por hectare (`Kg MV/ha`) utilizando o conjunto de dados **MontadoDB**.

A análise foi organizada nas seguintes etapas:

## 1. Carregamento e análise inicial dos dados

Inicialmente, o conjunto de dados é carregado e inspecionado para compreender sua estrutura, número de observações, variáveis disponíveis e características da variável-alvo `Kg MV/ha`.

Nesta etapa são realizadas:

* importação das bibliotecas;
* carregamento do MontadoDB;
* inspeção das dimensões e variáveis;
* análise descritiva da variável-alvo;
* visualização da distribuição de `Kg MV/ha`.

---

## 2. Preparação dos dados espectrais

Considerando as características do MontadoDB, são utilizadas informações espectrais provenientes de diferentes sensores.

As bandas espectrais são convertidas para suas respectivas escalas de reflectância antes do cálculo dos índices de vegetação.

São considerados principalmente dados provenientes de:

* Sentinel-2 (S2);
* Landsat 8 (L8);
* MODIS.

Devido às diferenças de resolução espacial e disponibilidade entre os sensores, é realizada uma preparação específica para permitir sua utilização na análise.

---

## 3. Construção dos índices de vegetação

A partir das bandas espectrais são calculados índices relacionados às características da vegetação.

### NDVI — Normalized Difference Vegetation Index

O NDVI é calculado a partir das bandas do vermelho e infravermelho próximo e é utilizado como indicador das características da vegetação.

São calculadas versões do NDVI para os sensores disponíveis.

### NDMI — Normalized Difference Moisture Index

Também é investigado o NDMI, construído a partir das bandas do infravermelho próximo e infravermelho de ondas curtas, permitindo incorporar informações relacionadas à umidade da vegetação.

---

## 4. Tratamento da qualidade das observações

Como as medições espectrais podem ser afetadas por condições atmosféricas, são identificadas observações potencialmente comprometidas pela presença de nuvens.

A análise prioriza as observações consideradas adequadas após esse tratamento.

---

## 5. Análise da variável-alvo

A distribuição de `Kg MV/ha` é analisada antes do ajuste dos modelos.

Também é investigada a transformação logarítmica:

`logMV = log(Kg MV/ha)`

A transformação é avaliada com o objetivo de verificar se proporciona uma distribuição mais adequada da variável utilizada na regressão.

---

## 6. Modelos de regressão linear

A construção do modelo é realizada de forma incremental, permitindo avaliar a contribuição de diferentes componentes.

### 6.1 Índice espectral

Inicialmente, é investigada a relação entre a quantidade de matéria verde e os índices espectrais, incluindo NDVI e NDMI.

### 6.2 Efeito do pasto

A variável `Sample` é utilizada para identificar o pasto (`Paddock`) associado a cada observação.

Esse componente é posteriormente incorporado aos modelos como variável categórica.

### 6.3 Componente temporal

A data e o dia do ano (`DOY`) são utilizados para representar a sazonalidade das observações.

É construída a variável `season_day`, permitindo investigar possíveis variações temporais na quantidade de matéria verde.

### 6.4 Modelo completo

A especificação mais completa combina:

* índice espectral;
* pasto (`Paddock`);
* componente sazonal;
* termo quadrático do componente temporal;
* interação entre o índice espectral e o período do ano.

Dessa forma, a análise avalia se a estimativa de matéria verde pode ser explicada não apenas pelas informações espectrais, mas também pelas diferenças entre pastos e pela sazonalidade.

---

## 7. Verificação dos modelos

Os modelos são acompanhados pela análise de seus resíduos e dos pressupostos associados à regressão linear.

São utilizadas análises gráficas e testes estatísticos para investigar aspectos como:

* distribuição dos resíduos;
* relação entre resíduos e valores ajustados;
* normalidade;
* heterocedasticidade;
* autocorrelação.

---

## 8. Comparação dos modelos

Por fim, diferentes especificações de regressão são comparadas para verificar como a inclusão dos componentes de pasto e sazonalidade modifica o ajuste dos modelos.

A comparação permite analisar a evolução desde modelos mais simples, baseados apenas em uma variável espectral, até especificações que incorporam informações espaciais e temporais do conjunto de dados.

---

## Fluxo geral

**MontadoDB**

↓

**Análise descritiva**

↓

**Normalização das bandas espectrais**

↓

**Cálculo de NDVI e NDMI**

↓

**Tratamento de observações potencialmente afetadas por nuvens**

↓

**Análise de `Kg MV/ha` e transformação logarítmica**

↓

**Regressão com índice espectral**

↓

**Inclusão do efeito de Paddock**

↓

**Inclusão da sazonalidade**

↓

**Diagnóstico dos resíduos**

↓

**Comparação dos modelos**

↓

**Resultados finais**
