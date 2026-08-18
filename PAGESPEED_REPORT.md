# Relatório PageSpeed Insights — silvasatin.adv.br

Gerado em 2026-08-17 via PageSpeed Insights API (Lighthouse 13.4.1).

## Scores atuais

| Categoria | Mobile | Desktop |
|---|---|---|
| Performance | 71 | 99 |
| Accessibility | 92 | 92 |
| Best Practices | 100 | 100 |
| SEO | 100 | 100 |

Este é o site com mais pontos de melhoria: performance mobile crítica (71, LCP de 5.6s) e dois problemas reais de acessibilidade.

## Problemas encontrados (por prioridade)

### 1. ✅ Corrigido — Embed do Google Maps carregando de forma síncrona/eager (causa raiz do LCP de 5.6s no mobile)
O maior vilão de performance: o mapa embutido carrega scripts da API do Google Maps (`places.js`, `main.js`, `init_embed.js`, `common.js`, `util.js`) somando **~230 KiB de JavaScript não utilizado no carregamento inicial**, disputando banda e main thread com o conteúdo real da página.

**Correção (prioridade máxima):** trocar o embed ativo por um "carregar mapa sob demanda" — mostrar uma imagem estática leve (ou um placeholder com o endereço) e só carregar o iframe/script do Google Maps quando o usuário clicar ("Ver mapa"/"Ver no Google Maps"), ou usar `loading="lazy"` num `<iframe>` do Google Maps Embed API (que não exige JS pesado) em vez do JS SDK completo.

**Implementado:** o `<iframe>` do Google Maps foi removido do carregamento inicial. Em seu lugar, `#local` mostra um card estático (`.map-placeholder`) com o endereço, um botão "Ver mapa" (que injeta o `<iframe>` via JS somente no clique) e um link "Ver no Google Maps ↗" que abre o endereço direto no Google Maps em nova aba. Nenhum recurso do Maps é buscado no carregamento da página.

### 2. ✅ Corrigido — Contraste de cor insuficiente (Accessibility, WCAG AA)
A cor dourada da marca `#a97e34` usada em textos (eyebrows, links "Falar sobre meu caso →") tem contraste de apenas 3.2–3.67:1 contra fundos claros (`#fbf6ea`, `#f6efe0`, `#ffffff`). O mínimo exigido é 4.5:1 para texto normal.

**Correção:** escurecer o dourado para um tom que atinja 4.5:1 nesses fundos (ex.: algo próximo de `#8a6428` ou mais escuro, testar com uma ferramenta de contraste) — aplicar em `styles.css` nas classes `.eyebrow` e `.card .more`.

**Implementado:** `--gold-deep` em `styles.css` alterado de `#a97e34` para `#8a6428`. Contraste calculado pela fórmula de luminância relativa do WCAG: 4.95:1 sobre `#fbf6ea`, 4.66:1 sobre `#f6efe0` e 5.34:1 sobre `#ffffff` — acima do mínimo de 4.5:1 em todos os fundos.

### 3. ✅ Corrigido — Falta de landmark `<main>` (Accessibility)
O documento não tem um elemento `<main>`, dificultando navegação por leitores de tela.

**Correção:** envolver o conteúdo principal (entre header e footer) em uma tag `<main>` em `index.html`.

**Implementado:** todo o conteúdo entre `<header>` e `<footer>` (seções hero, áreas, sobre, local e contato) agora está dentro de `<main>`.

### 4. ✅ Corrigido — Logo superdimensionado (~14 KiB de desperdício)
`assets/logo.webp` é servido em 900×900 para exibição de apenas 77×77 no rodapé.

**Correção:** gerar uma versão do logo já no tamanho de exibição (ou próximo, com folga para retina 2x).

**Implementado:** `assets/logo.webp` redimensionado de 900×900 (14,2 KiB) para 200×200 (5,1 KiB) via ImageMagick — folga suficiente para retina 2x sobre a exibição de 77×77.

### 5. Mapa estático do Google (fallback) sem compressão moderna (~4.6 KiB)
Não aplicável mais: com a correção do item 1, o mapa deixou de carregar qualquer imagem/iframe estático no carregamento inicial — só é buscado sob demanda, quando o usuário clica em "Ver mapa".

### 6. CSS e fontes render-blocking (~300ms) e cache lifetimes (~15-23 KiB)
`styles.css` e a fonte `Marcellus`/`Inter` do Google Fonts bloqueiam o render inicial.

**Correção:** usar `<link rel="preconnect">` para `fonts.googleapis.com`/`fonts.gstatic.com` (se ainda não houver), considerar `font-display: swap` (já comum, verificar se está ativo) e inlinar o CSS crítico.

**Status:** parcialmente já resolvido — `index.html` já tem `<link rel="preconnect">` para `fonts.googleapis.com` e `fonts.gstatic.com`, e a URL do Google Fonts já usa `display=swap`. Inlinar CSS crítico não foi feito nesta rodada por ser uma mudança mais arriscada num site sem processo de build (risco de divergência entre o crítico inline e `styles.css`); fica como próximo passo, se necessário, medindo o ganho real após os itens 1-4.

## Meta
O item 1 (mapa) é isoladamente o maior ganho possível — deve tirar segundos do LCP mobile. Os itens 2 e 3 são baratos e resolvem accessibility 92 → 100. Combinados, aproximam o site da perfeição em todas as categorias.
