# CLAUDE.md — Site Maryelle Ferreira (Nutricionista Clínica)

Este arquivo documenta as decisões de arquitetura e de Design System do
projeto. Ele existe para que qualquer pessoa (ou qualquer sessão futura de
IA) que trabalhe neste código entenda **por que** as coisas são como são —
e para impedir que o sistema visual seja alterado por engano.

> ⚠️ **Regra do projeto:** a identidade visual abaixo foi aprovada pela
> Maryelle. **Não altere cores, tipografia, espaçamentos, radius ou
> sombras sem aprovação explícita dela.** Se um componente novo "quase
> serve" mas quebra um destes tokens, ajuste o componente — nunca o token.

---

## 1. Stack técnica

| Camada | Escolha | Motivo |
|---|---|---|
| Framework | Next.js 16 (App Router) | SSR/SSG, performance, é o padrão atual do ecossistema React |
| Linguagem | TypeScript | segurança de tipos |
| Estilo | Tailwind CSS v4 (CSS-first, via `@theme`) | tokens versionados como código |
| Componentes | Padrão shadcn/ui (cva + Radix + `cn()`), **implementado manualmente** | ver nota abaixo |
| Ícones | lucide-react | biblioteca padrão do shadcn |
| Fontes | `next/font/local`, autohospedadas em `/public/fonts` | ver nota abaixo |

### Nota — shadcn/ui sem o CLI
O CLI oficial (`npx shadcn@latest init/add`) faz requisições para
`ui.shadcn.com`, domínio que não está liberado no ambiente onde este
projeto foi criado. Por isso, os componentes em `components/ui/` foram
escritos manualmente seguindo exatamente o mesmo padrão do shadcn
(`class-variance-authority` para variantes, Radix UI para primitivos
acessíveis, `cn()` para merge de classes). O arquivo `components.json`
foi mantido para compatibilidade — se `ui.shadcn.com` for liberado no
futuro, o CLI pode ser usado normalmente a partir daqui.

### Nota — fontes autohospedadas
Pelo mesmo motivo (rede restrita), as fontes não usam `next/font/google`
(que busca em `fonts.googleapis.com`). Os arquivos `.ttf` variáveis
oficiais foram baixados do repositório público `google/fonts` e vivem em
`public/fonts/`, carregados via `next/font/local`. Isso também traz um
benefício de performance/privacidade: zero requisição a servidores do
Google em produção.

---

## 2. Direção visual — "Terracota Editorial", paleta "Vermelho Seco" (v2)

Conceito: estética editorial, próxima de revista de lifestyle premium —
sofisticação pela tipografia e pelo espaço em branco, não por ornamentos.
Em 2026-08, a paleta evoluiu: o **vermelho seco** passou a ser a cor de
**personalidade principal** da marca (antes era um detalhe raro de
assinatura) — usado em blocos geométricos assimétricos, nunca preenchendo
uma seção inteira. O carvão continua como estrutura/texto/botão primário.

**O que evitar sempre:** verde (qualquer tom), visual fitness, aparência
de clínica/hospital, excesso de elementos, templates genéricos, excesso
de rosa, estética infantil, fontes muito delicadas, botões em pílula,
sombras pesadas, seções inteiras preenchidas de vermelho.

## 3. Cores

Definidas em `app/globals.css`, expostas como utilitários Tailwind
(`bg-carvao`, `text-vermelho`, etc.):

| Token | Hex | Papel |
|---|---|---|
| `--color-carvao` | `#1C1B1A` | estrutura, texto principal, botão primário |
| `--color-vermelho-escuro` | `#6B2419` | blocos grandes, CTA final |
| `--color-vermelho` | `#A63A2C` | **cor principal de personalidade** — blocos, botões de ação, detalhes |
| `--color-vermelho-nude` | `#E7C4B8` | badges, avatares placeholder, detalhes discretos |
| `--color-terracota` | `#C89684` | acento secundário legado — uso pontual opcional |
| `--color-off-white` | `#F7F3EE` | fundo base |
| `--color-creme` | `#F1E9DD` | fundo alternativo quente |
| `--color-bege` | `#E7DDD2` | fundo alternativo / seções secundárias |
| `--color-marrom` | `#5C4A3E` | texto secundário/muted |

Neutros derivados do carvão (bordas, texto sutil):
`--color-carvao-70` `#4A4846` · `--color-carvao-40` `#8F8C89` ·
`--color-carvao-15` `#D8D5D1`.

**Regra de uso do vermelho:** cor de personalidade da marca — aparece em
blocos geométricos assimétricos (ex.: atrás da foto do Hero), botões de
ação e detalhes (badges, marcadores, links). Nunca preenche uma seção
inteira de fundo a fundo; o equilíbrio vem do off-white/creme/bege ao
redor. `--color-vermelho-escuro` é reservado a blocos grandes de
contraste, como o CTA final.

## 4. Tipografia

- **Display (títulos, h1–h4):** Cormorant Garamond — serifa com presença,
  usada com peso 500 e leve `letter-spacing` negativo.
- **Corpo (texto, UI):** Manrope — sans geométrica leve, peso 300 como
  padrão do body.

### Escala e hierarquia (tokens `--text-*` em `globals.css`)

| Utilitário | Tamanho | Uso |
|---|---|---|
| `text-display-1` | 3.5rem (mobile: usa `display-2`) | H1 do Hero |
| `text-display-2` | 2.75rem | H1 de páginas internas |
| `text-h2` | 2.125rem | seções principais |
| `text-h3` | 1.5rem | subseções |
| `text-h4` | 1.25rem | títulos de card |
| `text-body-lg` | 1.125rem | introduções de seção |
| `text-body` | 1rem | texto corrido |
| `text-small` | 0.875rem | legendas, apoio |
| `text-caption` | 0.75rem | metadados |

`h1`–`h4` já vêm estilizados globalmente em `globals.css` — não é
necessário aplicar classe de fonte manualmente em títulos.

## 5. Espaçamento, grid e responsividade

- Container editorial: classe `.container-editorial`, `max-width: 1200px`,
  padding lateral de `1.25rem` no mobile e `2.5rem` a partir de `768px`.
- Breakpoints seguem o padrão Tailwind (`sm`, `md`, `lg`, `xl`).
- Abordagem **mobile-first**: estilos base pensados para mobile, ajustes
  de escala (ex.: `h1`) entram via `md:`.
- Grid de conteúdo (cards, serviços) usa `grid` do Tailwind
  (`grid-cols-1 md:grid-cols-3`, etc.) — sem biblioteca externa de grid.

## 6. Border radius

Cantos **levemente arredondados — nunca em pílula**:

| Token | Valor | Uso |
|---|---|---|
| `--radius-sm` | 0.25rem | badges, foco de teclado |
| `--radius-md` | 0.375rem | botões, inputs |
| `--radius-lg` | 0.5rem | cards |

## 7. Sombras

Muito sutis por padrão — a profundidade vem do tom da cor, não do blur:

- `--shadow-soft`: repouso de cards.
- `--shadow-elevated`: hover de elementos interativos ou modais, se
  necessários no futuro.

## 8. Componentes (`components/ui/`)

| Componente | Arquivo | Notas |
|---|---|---|
| `Button` | `button.tsx` | variantes `primary` / `secondary` / `accent` / `signature` / `ghost`; tamanhos `sm` / `default` / `lg` |
| `Card` | `card.tsx` | sem borda; sombra suave; `CardHeader/Title/Description/Content/Footer` |
| `Input` | `input.tsx` | foco em terracota (não vermelho — vermelho fica reservado ao foco de teclado geral e a CTAs) |
| `Label` | `label.tsx` | via Radix `Label` |
| `Badge` | `badge.tsx` | variantes `neutral` / `accent` / `signature` / `outline` — para eyebrows e tags ("Presencial", "Online") |

Links de texto corrido (fora de botão) usam a classe utilitária
`.text-link` (definida em `globals.css`): sublinhado em terracota que vira
vermelho no hover.

## 9. Estados interativos

Aplicados de forma consistente em todos os componentes:

- **Hover:** leve mudança de tom (nunca troca de cor completa) +, no botão
  `primary`, elevação sutil de sombra.
- **Focus:** `outline` de 2px em `--color-vermelho` com `offset` de 2px —
  **visível em toda a interface**, nunca removido (acessibilidade).
- **Active:** escurece levemente e aplica `scale-[0.98]` nos botões
  sólidos, para dar resposta tátil ao clique.
- **Disabled:** opacidade 40% + `pointer-events: none`.

## 10. Movimento

Fade suave e lento, sem "salto" — condizente com o conceito editorial.
`prefers-reduced-motion: reduce` é respeitado globalmente
(`globals.css`), reduzindo todas as animações a ~0 para quem tiver essa
preferência ativada no sistema.

## 11. Estrutura de pastas

```
app/
  layout.tsx          → fontes, metadata, wrapper HTML
  page.tsx             → Home definitiva (compõe todas as seções)
  globals.css          → todos os tokens do Design System + estilos base
components/
  ui/                  → Button, Card, Input, Label, Badge (base reutilizável)
  sections/             → Navbar, Hero, Sobre, Abordagem, Consultas,
                          Diferenciais, ComoFunciona, Depoimentos, FAQ,
                          CTAFinal, Footer
public/
  fonts/               → Cormorant Garamond e Manrope autohospedadas
  images/               → hero-maryelle.jpg (Hero + Sobre), avatar-maryelle.jpg (Depoimentos)
lib/
  utils.ts             → cn()
components.json         → config shadcn/ui (para uso futuro do CLI, se a rede permitir)
CLAUDE.md                → este arquivo
```

## 12. Home e páginas internas — implementado (2026-08-24)

**Home (`/`):** todas as 11 seções aprovadas em `components/sections/`,
compostas em `app/page.tsx`.

**Páginas internas**, todas usando Navbar/Footer/CTAFinal compartilhados e
o mesmo Design System (nada de paleta, tipografia ou componentes
diferentes):
- `/sobre` — biografia, formação, forma de pensar (frases próprias dela)
- `/consultas` — os 3 formatos de atendimento + diferenciais
- `/como-funciona` — os 4 passos do acompanhamento, com mais detalhe que a versão resumida da Home
- `/faq` — perguntas frequentes (inclui 1 pergunta a mais que a versão da Home)
- `/contato` — formulário visual (Nome, E-mail, Formato, Mensagem); **ainda sem canal de contato real conectado** — falta WhatsApp/e-mail/endereço/link de agenda reais para tornar funcional

`CTAFinal` agora aceita `title`/`description` opcionais e sempre linka para
`/contato`. `Navbar` linka para as páginas reais (`/sobre`,
`/como-funciona`, `/consultas`, `/faq`) e mantém `#depoimentos` como
âncora de volta à Home, já que depoimentos só existem lá.

- `npm run build` e `npx eslint .` rodados sem erros ou avisos em todas as
  9 rotas.
- Nenhuma informação foi inventada: preços, endereço, telefone e horários
  não aparecem em nenhuma página por não terem sido fornecidos.

## 13. WhatsApp — configuração centralizada (2026-08-24)

**Fonte única do número:** `lib/whatsapp.ts` — `WHATSAPP_NUMBER =
"5562994935712"`. Nunca duplicar esse número em nenhum componente; sempre
importar de `lib/whatsapp.ts`.

**Componente padrão:** `components/ui/whatsapp-button.tsx` (`WhatsAppButton`)
— todo CTA de WhatsApp do site usa esse componente, nunca um `<a>` cru com
o número. Ele já garante `target="_blank"`, `rel="noopener noreferrer"` e
altura mínima de 44px (toque adequado no mobile).

**Mensagens pré-preenchidas** (`WhatsAppMessageKey`):
- `"agendar"` — Navbar, Hero, cards de Consultas, página de Contato
- `"conhecer"` — CTA intermediário de Como Funciona
- `"final"` — CTAFinal (usado em todas as páginas)

**Pontos de uso confirmados:** Navbar, Hero, seção/página Consultas,
seção/página Como Funciona (CTA intermediário), CTAFinal (todas as
páginas) e página Contato (CTA principal, com formulário visual como
alternativa secundária).

**Verificação rodada:** busca por todas as ocorrências do número
confirmou que ele existe em um único arquivo; nenhum número antigo,
fictício ou duplicado foi encontrado; todos os links `wa.me` testados
geram a mensagem correta ao decodificar a URL; build e lint sem erros em
todas as 9 rotas.

## 15. Auditoria mobile (2026-08-24)

Problemas encontrados e corrigidos:

1. **Crítico — sem menu mobile:** os links de navegação (`Sobre`, `Como
   funciona`, `Consultas`, `Depoimentos`, `FAQ`) estavam com `hidden
   md:flex` e não tinham nenhuma alternativa no mobile — ficavam
   inacessíveis. Criado `components/sections/mobile-menu.tsx` (client
   component) com botão hambúrguer (44×44px) e painel dropdown.
2. **Navbar apertada em telas de 320–375px:** logo + botão de WhatsApp
   podiam ficar espremidos. Logo agora é responsiva (`text-base` →
   `text-lg` → `text-xl`) e o texto do botão abrevia para "Agendar" abaixo
   de `sm`, mantendo sempre 44px de altura.
3. **Bloco de vermelho seco ausente no mobile:** em `Hero` e `PageHero` o
   acento era `hidden md:block` — no mobile a marca perdia a personalidade
   visual. Adicionado um acento mobile-specific (preso ao wrapper da foto
   no Hero, para nunca sobrepor o texto; uma faixa lateral discreta no
   PageHero).
4. **Grid "Minha abordagem" 2×2 apertado:** em telas ≤375px o texto
   "Individualidade" ficava espremido. Mudado para 1 coluna em mobile
   pequeno, 2 a partir de `sm`, 4 no desktop; padding reduzido no mobile.
5. **Texto pequeno demais:** detalhes em "Minha abordagem" e "Como
   funciona" estavam em `text-xs` (12px); subidos para `text-small`
   (14px), mais legível no mobile.
6. **FAQ — "+" desalinhado em perguntas de 2–3 linhas:** trocado
   `items-center` por `items-start` no `<summary>`, tanto na seção da Home
   quanto na página `/faq`.
7. **Botões dos cards de Consultas pequenos para o polegar:** agora
   `w-full` no mobile, `w-auto` a partir de `sm`.

**Sem problemas encontrados em:** cortes de fotografia (Hero, Sobre — a
proporção do crop é fixa via `aspect-[...]`, então a foto não corta de um
jeito diferente/pior no mobile, só reduz de tamanho), overflow horizontal
(nenhuma largura fixa fora de `hidden md:block`), animações (todas em
CSS, sem JS de scroll pesado, e `prefers-reduced-motion` já respeitado
globalmente).

Build e lint seguem limpos em todas as 9 rotas após as correções.

## Animações e refinamento visual (2026-08-24)

**Filosofia:** sutil, rápida (~200–620ms), sem "salto", sem parallax.
Tudo respeita `prefers-reduced-motion` — a regra global em `globals.css`
zera `animation-duration`, `transition-duration` e `transition-delay`
para quem prefere menos movimento (o conteúdo ainda aparece, só sem a
animação).

**Peças novas:**
- `components/ui/reveal.tsx` — componente `<Reveal>` (client component,
  `IntersectionObserver`) que anima fade + leve `translateY` na entrada
  em viewport. Dispara uma única vez. Aceita `delay` (ms) para criar
  cascata entre itens irmãos (cards, pilares, passos, depoimentos, FAQ).
- `hooks/use-reveal.ts` — mesma lógica como hook, para elementos que não
  podem ser envolvidos por uma `<div>` sem quebrar o HTML (ex.: `<li>`
  dentro de `<ul>`). Usado em `components/ui/diferencial-item.tsx`.
- Classes utilitárias em `globals.css`: `.reveal`/`.reveal-visible`
  (entrada), `.photo-frame` (zoom sutil de 3.5% na foto ao passar o
  mouse — só em dispositivos com hover real, nunca no toque),
  `.card-lift` (elevação de 3px + sombra no hover dos cards),
  `.decorative-dot` (usado com `group-hover:scale-*` do Tailwind nos
  marcadores, número dos passos e ícone de aspas dos depoimentos).

**Onde foi aplicado:** todas as seções da Home e das páginas internas
(Hero, Sobre, Abordagem, Consultas, Diferenciais, Como Funciona,
Depoimentos, FAQ, CTA final, formulário de Contato) — cascata leve entre
itens irmãos (60–100ms de diferença cada). Fotos do Hero e de Sobre têm
zoom sutil no hover. Cards (`components/ui/card.tsx`) elevam levemente no
hover por padrão. Botões primary/secondary ganharam um `translateY(-1px)`
sutil no hover. O link de texto (`.text-link`) trocou o sublinhado
estático por um sublinhado que cresce da esquerda para a direita no
hover. O menu mobile agora abre/fecha com transição de altura suave
(grid-template-rows) e o ícone hambúrguer gira para um X.

Build e lint seguem limpos em todas as 9 rotas.

## Auditoria de SEO (2026-08-24)

**⚠️ Ação pendente antes de publicar:** `lib/seo.ts` define `SITE_URL`
como o placeholder `https://seudominio.com.br`. Isso afeta canonical,
sitemap.xml, robots.txt e Open Graph. Assim que o domínio real for
definido, trocar o valor (ou setar a env var `NEXT_PUBLIC_SITE_URL`) —
sem isso, os canonicals e o sitemap apontam para um domínio que não existe.

**Title / meta description:** cada página tem título e descrição únicos,
alinhados a um dos termos-alvo (nutricionista em Goiânia, nutricionista
clínica em Goiânia, consulta nutricional em Goiânia, nutricionista
online, acompanhamento nutricional) — sem repetição forçada de palavra-
chave. `layout.tsx` define um `title.template` (`%s | Maryelle Ferreira`)
que as páginas internas herdam automaticamente; a Home precisa do título
completo por extenso porque a rota raiz não herda o template do layout
(comportamento do Next.js).

**Headings:** um único H1 por página (confirmado via teste). Corrigido um
pulo de hierarquia em `/consultas` (ia de H1 direto para H3 dos cards) —
adicionado H2 "Escolha o formato ideal para você" antes da grade de cards.

**Canonical:** definido em todas as páginas via `alternates.canonical`.

**Open Graph / Twitter:** `app/opengraph-image.tsx` gera uma imagem de
1200×630 no padrão visual da marca (fundo off-white, bloco vermelho seco,
nome e tagline) — usada automaticamente como `og:image` e `twitter:image`
em todas as páginas. Twitter card = `summary_large_image`.

**Sitemap e robots:** `app/sitemap.ts` e `app/robots.ts` geram
`/sitemap.xml` e `/robots.txt` dinamicamente com todas as 6 rotas.

**Dados estruturados (JSON-LD):**
- `components/seo/business-json-ld.tsx` — schema.org `Nutritionist`/
  `MedicalBusiness`, presente em todas as páginas (via layout). Só inclui
  o que foi informado: nome, Goiânia/GO/BR, telefone (mesmo nº de
  WhatsApp), Instagram. **Sem** endereço completo, CRN ou faixa de preço
  — não foram fornecidos.
- `components/seo/faq-json-ld.tsx` — schema.org `FAQPage`, usado na Home
  e em `/faq`. As perguntas vêm de uma fonte única (`lib/faq-data.ts`)
  compartilhada com o conteúdo visível, para nunca divergir do que a
  pessoa vê na tela (exigência do Google para esse tipo de marcação).

**Favicon:** `app/icon.tsx` gera um favicon de marca (monograma "MF" em
carvão), substituindo o ícone genérico padrão do Next.js.

**Alt text:** revisado para ser descritivo e mencionar contexto (cargo,
cidade) apenas onde soa natural — sem repetir a mesma frase-chave em
todas as imagens.

**Performance:** imagens já otimizadas via `next/image` (AVIF/WebP,
tamanhos responsivos); fontes autohospedadas via `next/font/local`
(zero requisição externa de fonte); `opengraph-image` e `icon` rodam em
runtime Node (não Edge — evita aviso de depreciação do Next 16) e são
pré-renderizados como conteúdo estático no build.

**Não incluído (por não ter sido fornecido ou não se aplicar):**
endereço completo do consultório, CRN, preços, Google Business Profile
(perfil separado do Google, não faz parte do código do site).

## Regras para qualquer alteração futura

1. Nunca introduzir verde, tons fitness/clínicos, botões em pílula ou
   sombras pesadas.
2. O vermelho de assinatura é escasso por design — se parecer "decoração",
   está sendo usado errado.
3. Não instalar componentes de UI adicionais sem necessidade clara ligada
   ao conteúdo real de uma página aprovada.
4. Qualquer mudança nos tokens de `globals.css` precisa de aprovação
   explícita da Maryelle antes de ser aplicada.

## Auditoria técnica — acessibilidade e performance (2026-08-24)

**Contraste (WCAG):** calculei a razão de contraste real de cada
combinação de cor do Design System. Dois problemas encontrados e
corrigidos:
- `--color-carvao-40` (usado em `--foreground-subtle`, texto secundário
  pequeno) tinha só **3.03:1** contra o fundo — abaixo do mínimo de
  4.5:1 para texto normal. Escurecido para `#6b6864` → agora **4.98:1**.
  O valor antigo foi preservado em `--color-carvao-30`, reaproveitado
  só para bordas de campo de formulário.
- Borda padrão dos `Input`/`textarea` (`--border-color`, usada também em
  divisórias decorativas) tinha só **1.32:1** — muito abaixo do mínimo
  de 3:1 que o WCAG exige para limites de componentes de interface
  (SC 1.4.11). Criado `--border-strong` (`border-border-strong`),
  aplicado só nos campos de formulário — as divisórias decorativas
  (header, FAQ, menu mobile) continuam sutis de propósito.
- Texto sobre o bloco vermelho destacado em "Minha abordagem" estava a
  85% de opacidade (contraste 4.65:1, no limite); subido para 95%
  (contraste mais confortável).

**Skip link:** adicionado "Pular para o conteúdo" (`.skip-link` em
`globals.css`, renderizado no início da `Navbar`) — invisível até
receber foco pelo teclado, permite pular a navegação repetida em todas
as páginas. Cada `<main>` agora tem `id="conteudo"`.

**Já estava correto (verificado, sem necessidade de mudança):**
- `:focus-visible` visível globalmente, nunca removido.
- Tamanho de toque: todos os CTAs de WhatsApp e o botão do menu mobile
  já tinham 44px (corrigido na auditoria mobile anterior).
- `aria-label` em elementos ícone-only (menu mobile, botões de
  WhatsApp); elementos puramente decorativos (blocos vermelhos, ícone de
  aspas, marcadores) com `aria-hidden`.
- Semântica: `<header>`/`<nav>`/`<main>`/`<footer>`, FAQ com
  `<details>/<summary>` nativos (navegável por teclado sem JS extra),
  `<Label htmlFor>` associado a cada campo.
- `prefers-reduced-motion` já zera duração e delay das transições.

**Performance — imagens:** as duas fotos-fonte estavam em resolução
muito acima do necessário (hero: 4284×5712, 2MB · avatar: 3648×3648,
1,5MB) — nenhuma tela do site exibe além de ~360px de largura, nem em
retina. Redimensionadas para 1200×1600 e 1000×1000 (ainda generosas
para retina 3x), qualidade JPEG 85 progressivo: **de 3,6MB para ~400KB
juntas, sem perda perceptível**. O `next/image` já gera AVIF/WebP e
tamanhos responsivos automaticamente em cima desses arquivos.

**Performance — fontes:** já autohospedadas via `next/font/local` com
`display: swap` (zero requisição externa, ajuste automático de fallback
para reduzir CLS).

**Performance — JavaScript:** todo o JS de todas as rotas somado dá
~650KB não comprimido (React + Next + todo o app) — leve. Único uso de
`IntersectionObserver` é o `<Reveal>`/`useReveal`, com `disconnect()` no
cleanup; nenhuma lib pesada nas dependências.

**Core Web Vitals (avaliação sem Lighthouse real):**
- LCP: foto do Hero com `priority` (pré-carrega, não espera lazy-load).
- CLS: todas as imagens usam `fill` dentro de contêiner com
  `aspect-ratio` fixo — sem "pulo" de layout ao carregar.
- Fontes locais com fallback ajustado — sem CLS perceptível de fonte.

Build e lint seguem limpos em todas as rotas.

## Auditoria de UX e conversão (2026-08-24)

Análise da Home pela perspectiva de um visitante que não conhece a
Maryelle. Problemas encontrados e corrigidos:

1. **"Para quem é" não existia em lugar nenhum da Home** — falava-se de
   *como* (ciência, individualidade, estratégia) mas nunca de *para
   quem*. Corrigido reescrevendo a subheadline do Hero para liderar com
   "Para quem quer emagrecer e mudar hábitos de verdade..." — baseado em
   temas já aprovados na marca (emagrecimento, composição corporal,
   mudança de hábitos).
2. **Preço era um vácuo na Home** — a nota de transparência ("valores
   sob consulta") só existia na página `/consultas`, não na seção
   equivalente da Home. Adicionada a mesma nota também ali.
3. **FAQ não respondia às 2 objeções mais prováveis de quem acabou de
   chegar** ("isso serve pra mim?" e "quanto custa?") — só respondia
   dúvidas de quem já tinha decidido agendar. Adicionadas as duas
   perguntas em `lib/faq-data.ts` (fonte única — atualiza Home, `/faq` e
   o JSON-LD ao mesmo tempo), como as duas primeiras da lista.
4. **Rodapé era um beco sem saída** — quem rolava a página inteira sem
   clicar em nenhum CTA não encontrava nem WhatsApp nem Instagram.
   Adicionados os dois no rodapé (o Instagram já era uma referência de
   marca existente, `@nutrimaryelleferreira`).

**Avaliado e mantido como está:** quantidade de CTAs (5 ao longo da
Home) — bem distribuídos, não empilhados, cada um com mensagem
contextual diferente; não configura "landing page agressiva". Próximo
passo é sempre claro e consistente (WhatsApp). Depoimentos são
anônimos por proteção à privacidade das pacientes — trade-off aceito,
não uma correção pendente.
