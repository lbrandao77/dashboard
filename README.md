# Dashboard — Indicadores de Fechamento Mensal de Inbound

Dashboard executivo de página única (`index.html`) para acompanhar a performance
dos canais de **Inbound** da The Foursales Company: contratações por canal, mix
Inbound vs Hunting vs Indicação, custos de mídia paga, tempos de fechamento e a
lista de vagas fechadas por Inbound.

## Como funciona

- **Single-file**: todo o app está em `index.html` (HTML + CSS + JS, sem build).
- **Dados ao vivo**: os números são lidos diretamente da planilha
  *Indicadores de fechamento mensal de Inbound* (Google Sheets) via CSV publicado
  e gviz. Os dados só são carregados quando o usuário clica em **Atualizar**.
- **Bibliotecas via CDN**: Chart.js e Lucide.

## Rodar localmente

Basta abrir o `index.html` no navegador, ou servir a pasta:

```bash
npx serve .
# ou
python -m http.server 8000
```

## Deploy

Site 100% estático — a Vercel serve o `index.html` da raiz sem nenhuma
configuração de build.
