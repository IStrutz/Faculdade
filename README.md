# Faculdade
---
title: "Isabel Strutz"
format: html
editor: visual
---

```{r}
#| label: load-packages
#| include: false

library(tidyverse)
library(WDI)
library(gganimate)
```

## Inflação

A inflação é o aumento generalizado e contínuo dos preços de bens e serviços em um país, medido geralmente por índices como o IPC (Índice de Preços ao Consumidor). Ela reduz o poder de compra da moeda e é um dos principais indicadores macroeconômicos observados por governos e bancos centrais para definir políticas econômicas.

## Inflação 2000-2023

Aqui temos um gráfico que mostra a inflação ano a ano para os 10 países mais populosos do mundo, entre 2000 e 2023.

A cada mudança de ano na animação, vemos como a inflação varia de país para país. Em alguns momentos ela sobe em todos os países, indicando crises globais, enquanto em outros vemos contrastes regionais.

```{r}
#| label: painel-inflacao
#| warning: false
#| echo: false


dadospainel <- WDI(country = 'all',
                   indicator = 'FP.CPI.TOTL.ZG',
                   start = 2000, end = 2023,
                   extra = TRUE)


top10_paises <- c("China", "India", "United States", "Indonesia", "Pakistan",
                  "Nigeria", "Brazil", "Bangladesh", "Russia", "Mexico")


dadospainel_filtrado <- dadospainel %>%
  filter(country %in% top10_paises)


ggplot(dadospainel_filtrado, aes(x = year, y = FP.CPI.TOTL.ZG, group = country, color = country)) +
  geom_line() +
  labs(y = "Inflação (% ao ano)", x = "Ano", title = "Inflação nos 10 países mais populosos") +
  transition_reveal(year)

```

## Melhor comparação

Esse gráfico mostra quem tem mais ou menos inflação em cada ano. Ele funciona como uma fotografia anual da economia global. Vemos, por exemplo, o Brasil subindo ou descendo no ranking ao longo do tempo, o que reflete os altos e baixos da inflação. É uma forma clara de comparar países lado a lado e entender como cada um responde a contextos econômicos diferentes

```{r}
#| label: transversal-inflacao
#| warning: false
#| echo: false


dados_animados <- WDI(country = 'all',
                      indicator = 'FP.CPI.TOTL.ZG',
                      start = 2000, end = 2023,
                      extra = TRUE)


top10_paises <- c("China", "India", "United States", "Indonesia", "Pakistan",
                  "Nigeria", "Brazil", "Bangladesh", "Russia", "Mexico")


dados_animados <- dados_animados %>%
  filter(country %in% top10_paises)


library(gganimate)

ggplot(dados_animados, aes(x = reorder(country, FP.CPI.TOTL.ZG), y = FP.CPI.TOTL.ZG, fill = region)) +
  geom_col(show.legend = FALSE) +
  coord_flip() +
  labs(title = "Inflação (% ao ano) - Ano: {closest_state}",
       x = "País",
       y = "Inflação (%)") +
  transition_states(year, transition_length = 2, state_length = 1) +
  ease_aes("cubic-in-out")

```

## Inflação no Brasil

Este gráfico mostra a evolução da inflação no Brasil ao longo dos últimos 23 anos.

Podemos observar picos, como durante crises econômicas e mudanças de governo, e também períodos de maior estabilidade.

Neste gráfico o pico mais acentuado acontece por volta de 2002/2003, provavelmente relacionados a fatores como a crise de energia no início dos anos 2000, as incertas políticas com a eleição de 2002 e a desvalorização cambial

É possível notar um período de inflação mais baixa por volta de 2007-2008, devido a crise financeira global.

A principal razão para a queda da inflação em 2017 foi a supersafra agrícola, que derrubou os preços dos alimentos. E além disso o alto desemprego que contribuiu para segurar os preços.

A inflação também atingiu um dos pontos mais baixos por volta de 2020-2021, em reflexo inicial da desaceleração econômica causada pela pandemia de COVID-19, que diminuiu a demanda em muitos setores.

```{r}
#| label: temporal-inflacao
#| warning: false
#| echo: false

library(gganimate)

dadostemporal <- WDI(country = 'BR',
                     indicator = 'FP.CPI.TOTL.ZG',
                     start = 2000, end = 2023)

ggplot(dadostemporal, aes(x = year, y = FP.CPI.TOTL.ZG)) +
  geom_line(color = "blue") +
  geom_point() +
  labs(y = "Inflação (% ao ano)", x = "Ano") +
  transition_reveal(year)

```
