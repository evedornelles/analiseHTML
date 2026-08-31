# Dashboard Comercial

Painel único em HTML pra acompanhar vendas do mês: faturamento, ticket médio, funil, top filiais, desempenho por vendedor etc. Sem backend, sem build, sem npm install, abre o arquivo e já funciona.

## Como usar

Abre o `studio-heeler-dashboard.html` em qualquer navegador. Só isso. Se quiser hospedar em algum lugar (Netlify, GitHub Pages, servidor interno), basta subir o arquivo, ele não depende de nada externo.

Os filtros no topo (período, vendedor, faixa de preço, canal) recalculam tudo na hora, KPIs, gráfico, ranking de filiais, tabela de vendedores, funil e formas de pagamento. Não é só cosmético, os números realmente batem entre si porque vêm da mesma base.

O botão de sol/lua no canto superior direito troca entre tema claro e escuro.

## De onde vêm os dados

Aqui vai o ponto mais importante: **os dados são simulados**. Não tem integração com nenhum sistema de vendas real. O que existe é um gerador (dentro do próprio `<script>`) que cria ~2.800 pedidos fictícios distribuídos entre 01/08 e 30/08/2026, com vendedor, filial, canal, faixa de preço, forma de pagamento e cliente atribuídos de forma ponderada (ex: e-commerce vende mais que loja física, PIX é o método mais usado, etc).

O gerador usa uma seed fixa, então os números são sempre os mesmos a cada vez que a página carrega, não é aleatório de verdade, é reprodutível. Isso foi proposital: facilita debugar e dá pra confiar que o print de ontem é igual ao de hoje.

Se for plugar em dados reais, o lugar pra mexer é a função que monta o array `orders` lá no início do script. A partir daí todo o resto (KPIs, gráfico, tabelas) já consome esse array e não precisa mudar nada.

## Métricas — como são calculadas

Nada aqui é hardcoded, tudo deriva do array de pedidos filtrado:

- **Faturamento / pedidos / ticket médio**: soma e contagem direta dos pedidos filtrados.
- **Taxa de recompra**: % de clientes (por `customerId`) com 2 ou mais pedidos dentro do recorte selecionado.
- **CAC / LTV**: CAC usa um orçamento de mídia mensal fixo (R$ 118k) rateado pelos dias selecionados, dividido pelos clientes novos no período. LTV é a receita média histórica por cliente que aparece no filtro atual.
- **Comparativo "vs. mês anterior"**: cada combinação de canal + faixa de preço tem um fator de crescimento próprio (e-commerce cresceu mais que B2B, por exemplo), calibrado pra que o total do mês bata em +14,8% — mas o número muda de verdade dependendo do que você filtra.
- **Funil de conversão**: só faz sentido pro canal e-commerce, então ele se desativa sozinho (com aviso) se você filtrar por loja física ou B2B.

## Estrutura do arquivo

Tudo num arquivo só, de propósito (fácil de compartilhar, sem dependência de build):

```
studio-heeler-dashboard.html
├── <style>   → tema dark/light via CSS variables, layout em grid
├── <body>    → markup dos cards, filtros, tabelas
└── <script>  → gerador de dados + funções de render (renderKPIs, renderChart, renderTopBranches...)
```

## Limitações conhecidas

- É tudo client-side. Se alguém abrir o "ver código-fonte" vai ver a lógica de geração de dados, não tem nada sensível ali, mas vale saber.
- O gráfico é SVG desenhado na mão (sem lib), então recursos mais avançados (zoom, pan, exportar imagem) não existem.

## Créditos

Desenvolvido por Évelim Dornelles.
