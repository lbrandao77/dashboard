# Dashboard - Indicadores de Fechamento Mensal de Inbound
## Contexto do Projeto

---

## O que e esse projeto

Dashboard visual para acompanhar a performance dos canais de Inbound da TFC
nos projetos de vagas fechadas, canceladas e divulgadas.

A planilha fonte se chama:

- **Nome:** Indicadores de fechamento mensal de Inbound
- **URL:** https://docs.google.com/spreadsheets/d/14DibweyDrXhIwS-s0lGmWhtFsWWtQkh-Neq5lUVBCOM/edit?usp=sharing
- **URL publicada:** https://docs.google.com/spreadsheets/d/e/2PACX-1vRMLzlrivbcCBvomqHmGR9PShELJ1mCl36FIpS3eMSNaVCbcj4ndebhb9tPEmMCz1WKevKLOMwScu7X/pubhtml
- **File ID:** 14DibweyDrXhIwS-s0lGmWhtFsWWtQkh-Neq5lUVBCOM

O dashboard deve priorizar uma leitura executiva: principais indicadores em
destaque na primeira visao e analises mais granulares em uma aba secundaria.

---

## Estrutura esperada do projeto

```text
index.html           <- dashboard completo, single-file, self-contained
CLAUDE.md            <- este arquivo, com contexto e regras de calculo
```

Observacao: no momento, o workspace contem este `CLAUDE.md`. Se o `index.html`
ainda nao existir, ele deve ser criado como arquivo unico em HTML/CSS/JS.

---

## Abas da planilha que alimentam o dashboard

O dashboard deve se alimentar das seguintes abas:

- `Dados gerais vagas Fechadas`
- `TFC - KPI´s Gerais 2026`
- `KPI´s de Canais do Inbound`
- `Extração planilha final`
- `Extração Inbound`

GIDs publicados:

```text
RPO                                -> 1036982827
Dados Vagas Divulgadas             -> 926531330
Dados gerais vagas Fechadas        -> 450756489
TFC - KPI´s Gerais 2026            -> 894469350
KPI´s de Canais do Inbound         -> 390003485
Extração planilha final            -> 1731679932
Extração Inbound                   -> 812389938
```

Padrao de export CSV publicado:

```text
https://docs.google.com/spreadsheets/d/e/2PACX-1vRMLzlrivbcCBvomqHmGR9PShELJ1mCl36FIpS3eMSNaVCbcj4ndebhb9tPEmMCz1WKevKLOMwScu7X/pub?gid={GID}&single=true&output=csv
```

Padrao de export CSV pelo link publico original:

```text
https://docs.google.com/spreadsheets/d/14DibweyDrXhIwS-s0lGmWhtFsWWtQkh-Neq5lUVBCOM/gviz/tq?tqx=out:csv&sheet={NOME_DA_ABA_URL_ENCODED}
```

O link original foi testado como publico e respondeu CSV sem autenticacao.

As abas mais importantes para performance por canal sao:

- `Extração Inbound`
- `Extração planilha final`
- `KPI´s de Canais do Inbound`
- `Dados gerais vagas Fechadas`, especialmente para identificar origem do contratado

---

## Contexto da planilha

A planilha consolida indicadores dos projetos de vagas rodados na TFC.

Cada projeto de vaga tem um ID de 4 digitos. Um mesmo ID pode representar mais
de uma posicao/credito dentro do projeto.

Exemplo: a vaga `4802` pode representar duas vagas de vendedor, para a mesma
posicao e regiao.

Na aba `Dados gerais vagas Fechadas`, as colunas `K` e `L` ajudam a identificar
essa estrutura. A coluna `PAI` e a principal regra:

```text
PAI = 0  -> linha consolidada do projeto
PAI = 1  -> primeiro contratado/primeira posicao do projeto
PAI = 2  -> segundo contratado/segunda posicao do projeto
PAI = 3+ -> demais contratados/posicoes
```

A linha `PAI = 0` e o consolidado dos indicadores do projeto como um todo.
As linhas `PAI >= 1` dizem respeito aos contratados/posicoes individuais.

Existem vagas fechadas com contratacao, canceladas, pausadas, fechadas sem
sucesso e outros status. Os calculos devem respeitar essa diferenca.

---

## Definicoes principais

### O que e Inbound

Inbound sao candidatos que chegam ate a empresa por divulgacao da vaga. E o
oposto operacional de hunting/captacao.

Canais de Inbound:

- Jobslot
- Indeed
- LinkedIn Timeline
- Software
- Repescagem
- Fonte de Talentos
- Meta Ads
- Tiktok Ads
- Jooble

### O que e Hunting / Captacao

Hunting ou captacao sao candidatos buscados ativamente por consultores via
LinkedIn, WhatsApp, banco de talentos, abordagens diretas etc.

Captacao nao deve ser classificada como Inbound.

### O que e Indicacao Interna do Cliente

Indicacao interna do cliente sao contratados indicados pela empresa contratante.
No dashboard, deve aparecer separada de Inbound e Hunting.

### O que e Acao Premium

Acao premium equivale a:

```text
Repescagem + Fonte de Talentos
```

Essas acoes devem alimentar indicadores como:

- Numero de vagas com acao premium
- Percentual de vagas com acao premium
- Aplicantes e finalistas gerados por acao premium

---

## Grupos de canais

Mesmo que o dashboard possa ignorar visualmente a divisao por grupos em alguns
graficos, o mapeamento deve estar preservado:

### Midia paga tradicional

- Indeed
- Meta Ads
- Jobslot

### Midia alternativa

- Repescagem
- Fonte de Talentos

### Midia organica

- Timeline LinkedIn

### Software

- Software
- Gupy
- Pandape

### Outros canais a mapear

- Tiktok Ads
- Jooble

---

## Regra mais importante: origem do contratado

Para saber o canal dos contratados, sempre usar a aba:

```text
Dados gerais vagas Fechadas
```

Campo principal:

```text
Origem do contratado
```

Se o valor for `-`, normalmente a linha e o PAI consolidado ou uma vaga
cancelada/sem contratacao. Nao contar `-` como contratado.

Essa coluna deve orientar os indicadores de:

- Contratados por canal de Inbound
- Contratados por Hunting
- Contratados por Indicacao Interna do Cliente
- Percentual Inbound vs Hunting vs Indicacao
- Tempo medio de fechamento por origem
- Contratados por unidade, quando combinado com a coluna de operacao/unidade

---

## Mapeamento de origem do contratado

### Inbound

```text
"Jobslot"              -> Jobslot
"Indeed"               -> Indeed
"Meta Ads"             -> Meta Ads
"Timeline LKD"         -> LinkedIn Timeline
"Timeline LinkedIn"    -> LinkedIn Timeline
"Software"             -> Software
"Gupy"                 -> Software
"Pandape"              -> Software
"Pandapé"              -> Software
"Repescagem"           -> Repescagem
"Fonte de Talentos"    -> Fonte de Talentos
"Jooble"               -> Jooble
"Tiktok Ads"           -> Tiktok Ads
```

### Hunting / Captacao

```text
"Captação SJ"             -> Hunting
"Captação EH"             -> Hunting
"Captação FS"             -> Hunting
"Captação SJ - Pandapé"   -> Hunting
"Captacao SJ"             -> Hunting
"Captacao EH"             -> Hunting
"Captacao FS"             -> Hunting
```

### Indicacao interna do cliente

```text
"Indicação do cliente"    -> Indicacao Interna do Cliente
"Indicacao do cliente"    -> Indicacao Interna do Cliente
```

### Sem contratacao

```text
"-"                       -> Sem contratacao / PAI / cancelada
"Sem sucesso"             -> Sem contratacao
```

---

## Indicadores principais da visao executiva

Esses devem ser os indicadores mais visuais e destacados do dashboard:

- Percentual de contratados Inbound vs Hunting vs Indicacao Interna do Cliente
- Quantidade de contratados por canal de Inbound
- Quantidade de contratados por Hunting
- Tempo medio de fechamento em vagas fechadas por Inbound
- Tempo medio de fechamento em vagas fechadas por Hunting
- Custo medio de midia paga
- Custo por contratacao de Inbound
- Media de aplicantes Inbound
- Media de finalistas Inbound
- Vagas divulgadas no mes
- Vagas novas divulgadas no mes

Importante: o percentual Inbound vs Hunting vs Indicacao deve considerar somente
projetos/vagas que tiveram contratacao.

---

## Indicadores que tambem devem existir

### Volume e funil por canal

- Quantidade de aplicantes por canal de Inbound
- Quantidade de finalistas por canal de Inbound
- Quantidade de contratados por canal de Inbound
- Aplicantes e finalistas de Tiktok Ads
- Aplicantes e finalistas de Jooble

### Vagas e cancelamentos

- Percentual de vagas canceladas
- Quantidade de vagas divulgadas no Inbound
- Quantidade de vagas divulgadas no mes
- Quantidade de vagas novas divulgadas no mes

### Custos

- Custo de midia paga, puxado da coluna `BF` da aba `Dados gerais vagas Fechadas`
- Custo de aquisicao de aplicantes
- Custo de aquisicao de finalistas
- Custo de contratacao do Inbound

### Medias

- Media de aplicantes de todos os canais Inbound por vaga
- Media de aplicantes por canal de Inbound
- Media de aplicantes de Captacao/Hunting
- Quantidade de aplicantes de Captacao/Hunting

### Primeira short

- Contratados por Inbound na primeira short
- Vagas fechadas por Inbound com candidatos da primeira short

### Unidade / consultoria

- Percentual de fechamento de Inbound em consultorias
- Contratados por unidade:
  - SJ
  - TFC RH
  - RPO

### Acoes premium

- Numero de vagas com acoes premium
- Percentual de vagas com acao premium

---

## Indicadores secundarios para aba de analise

Esses indicadores sao mais micros e devem aparecer em uma aba secundaria, por
exemplo `Analise detalhada`:

- Percentual de aplicantes de Inbound em vagas fechadas por Inbound
- Percentual de vagas em que rodamos Jobslot
- Percentual de vagas em que rodamos Indeed
- Percentual de vagas em que rodamos Meta Ads
- Percentual de cada canal dentro das contratacoes de Inbound
- Breakdown completo de aplicantes por canal
- Breakdown completo de finalistas por canal
- Breakdown completo de contratados por canal
- Comparacao de Inbound vs Hunting por unidade
- Tabela mensal completa

As porcentagens de presenca de canal devem ser calculadas em relacao ao total
de vagas fechadas.

---

## Fontes por aba

### `Dados gerais vagas Fechadas`

Fonte principal para identificar origem real do contratado, status, unidade,
tempo de processo, estrutura PAI e custo real de midia paga.

Campos importantes:

- ID da vaga
- Status
- Operacao/unidade
- PAI
- Rodou no Inbound?
- Short do contratado
- Data de abertura
- Data de fechamento
- Tempo de processo
- Numero de aplicantes total
- Numero de aplicantes de hunting/captacao
- Numero de finalistas total
- Numero de contratados total
- Origem do contratado
- Coluna `BF`: custo de midia paga

### `Extração Inbound`

Fonte granular por vaga para aplicantes e finalistas por canal.

Canais/pares esperados:

- Jobslot: aplicantes e finalistas
- Repescagem: aplicantes e finalistas
- Fonte de Talentos: aplicantes e finalistas
- Jobpost: aplicantes e finalistas, se existir
- Meta Ads: aplicantes e finalistas
- Software: aplicantes e finalistas
- Indeed: aplicantes e finalistas
- Tiktok Ads: aplicantes e finalistas
- Jooble: aplicantes e finalistas
- Mes

### `Extração planilha final`

Fonte consolidada de projetos fechados com dados de contratacao. Usar para
cruzar fechamento, origem e tempo medio, especialmente Inbound vs Hunting.

### `KPI´s de Canais do Inbound`

Fonte de totais consolidados por canal:

- Total de aplicantes por canal
- Total de finalistas por canal
- Total de contratados por canal
- Vagas fechadas por canal
- Custo de aquisicao de aplicante
- Custo de aquisicao de finalista
- Custo de contratacao

### `TFC - KPI´s Gerais 2026`

Fonte para KPIs mensais gerais:

- Vagas divulgadas no mes
- Vagas novas divulgadas no mes
- Vagas fechadas com e sem contratacao
- Vagas totais fechadas com contratacao
- Vagas canceladas
- Custo midia paga
- Custo de midia por vaga fechada no mes
- Tempo medio de fechamento da vaga

---

## Regras de calculo

### Contagem de contratados

Para contratados, usar linhas que representem contratacoes reais e tenham
`Origem do contratado` valida.

Nao contar:

- `-`
- `Sem sucesso`
- linhas sem origem
- vagas canceladas sem contratacao

### Percentual Inbound vs Hunting vs Indicacao

Considerar somente projetos/vagas com contratacao.

```text
% Inbound = contratados Inbound / total de contratados com origem valida
% Hunting = contratados Hunting / total de contratados com origem valida
% Indicacao = contratados Indicacao / total de contratados com origem valida
```

### Percentual de vagas canceladas

Usar total de vagas canceladas dividido pelo total de vagas fechadas/encerradas
do periodo, conforme disponibilidade da aba `TFC - KPI´s Gerais 2026` ou
`Dados gerais vagas Fechadas`.

### Custo de midia paga

Priorizar a coluna `BF` da aba `Dados gerais vagas Fechadas`.

Quando houver agregacao mensal ou total ja validada em `TFC - KPI´s Gerais 2026`,
usar como referencia de conferencia.

### Custo de contratacao do Inbound

```text
custo de contratacao inbound = custo total de midia paga / contratados inbound
```

### Custo de aquisicao de aplicantes

```text
custo por aplicante = custo total de midia paga / aplicantes inbound
```

### Custo de aquisicao de finalistas

```text
custo por finalista = custo total de midia paga / finalistas inbound
```

### Media de aplicantes Inbound por vaga

```text
media aplicantes inbound/vaga = total aplicantes inbound / total de vagas que rodaram Inbound
```

### Media por canal

```text
media aplicantes canal/vaga = aplicantes do canal / vagas em que o canal rodou
```

### Acao premium

Uma vaga teve acao premium se houve volume maior que zero em Repescagem ou Fonte
de Talentos.

```text
vagas com acao premium = vagas com Repescagem > 0 ou Fonte de Talentos > 0
% acao premium = vagas com acao premium / total de vagas analisadas
```

---

## Direcao visual do dashboard

O dashboard deve ser bem visual e separar claramente:

- Visao principal: KPIs executivos, graficos grandes e leitura rapida
- Analise detalhada: indicadores secundarios, percentuais de presenca, tabelas e breakdowns

Sugestoes de graficos:

- Donut ou barra 100% para Inbound vs Hunting vs Indicacao
- Barras horizontais para contratados por canal
- Linha mensal para vagas divulgadas, vagas novas e fechamentos
- Cards numericos para custos, medias e tempos
- Funil Aplicantes -> Finalistas -> Contratados por canal
- Tabela detalhada por canal com aplicantes, finalistas, contratados, conversao e custo

---

## Tecnologias sugeridas

- HTML/CSS/JS puro, sem framework
- Chart.js via CDN, se o arquivo puder depender de internet ao abrir
- Single-file sempre que possivel

---

## Backlog possivel

- Botao para atualizar dados via Google Sheets API
- Filtro por mes
- Filtro por unidade/consultoria
- Filtro por status de vaga
- Filtro por canal de Inbound
- Exportar dashboard como PDF
- Exportar tabela detalhada como CSV
