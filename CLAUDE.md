# CLAUDE.md — Site Maryelle Ferreira (Nutricionista Clínica)

> Este projeto foi recriado do zero em 2026-08-25. A versão anterior
> (Next.js + Tailwind v4 + shadcn manual) nunca chegou a ser publicada e
> foi descartada por decisão explícita da Maryelle — não reaproveite nada
> dela. A documentação abaixo descreve **apenas** o que existe agora.

## O que é

Uma **one page estática**: HTML + CSS puro. Sem framework, sem
JavaScript, sem build, sem backend, sem banco de dados, sem dependências
de npm. A prioridade combinada com a Maryelle é
**funcionar > ser bonito > ser complexo**.

## Estrutura

```
index.html      → a página inteira (todas as seções, nesta ordem):
                   Hero, Sobre mim, Como posso te ajudar, Como funciona,
                   Atendimentos, Frase de impacto, CTA final, Rodapé
css/style.css    → tokens de cor/tipografia + todo o estilo + responsividade
favicon.svg      → ícone da aba (monograma "MF" em terracota)
assets/img/      → fotos reais da Maryelle (ver assets/img/README.md)
README.md        → como rodar localmente e publicar na Vercel
```

Menu mobile é feito só com CSS (checkbox hack), sem JavaScript — zero
risco de erro de console relacionado a script.

## Identidade visual

- **Tipografia:** serifada (`Georgia`/stack de sistema) nos títulos, sans
  de sistema no corpo. Fontes de sistema de propósito — zero requisição
  externa, zero dependência de rede (o projeto anterior teve problemas
  justamente com dependências de rede em build).
- **Cores** (definidas em `:root` no `style.css`):
  - `--color-carvao` `#262220` — texto, estrutura, rodapé, seção de frase de impacto
  - `--color-vermelho` `#a63a2c` — cor de assinatura (CTAs, eyebrows, números das etapas)
  - `--color-vermelho-escuro` `#7a2a1f` — hover do vermelho / bloco do CTA final
  - `--color-off-white` `#faf6f1` — fundo base
  - `--color-bege` `#efe6d8` — fundo alternativo (seções pares)
- **Botões:** cantos levemente arredondados, nunca em pílula. Sem sombras pesadas.
- **Evitar sempre:** verde, visual fitness/clínico, excesso de elementos, pílulas, sombras pesadas.

## WhatsApp

Número: `5562994935712`. O link (com mensagem pré-preenchida) é o mesmo
em **todos** os botões de agendamento do site — Hero, cards de
Atendimento, CTA final e rodapé:

```
https://wa.me/5562994935712?text=Ol%C3%A1%2C%20Maryelle!%20Vim%20pelo%20seu%20site%20e%20gostaria%20de%20saber%20mais%20sobre%20as%20consultas.
```

Se o número ou a mensagem mudarem, é preciso atualizar manualmente em
cada ocorrência dentro do `index.html` (são 7 links idênticos) — não há
constante compartilhada porque o projeto não usa JavaScript/build.

## Título do Hero (2026-08-25)

O H1 do Hero é **"Nutrição que cabe na sua vida real."** — curto,
proposital, é o maior elemento visual da página (`--hero-title`, até
3.75rem no desktop). O subtítulo abaixo dele deve continuar curto (uma
frase) para não sobrecarregar a primeira seção.

## Fotos (2026-08-25)

O Hero e a seção Sobre têm espaços reservados para fotos reais da
Maryelle (`.hero-visual` e `.sobre-visual`) — hoje exibem um bloco
decorativo em CSS (não é um placeholder "quebrado", é a aparência padrão
até as fotos entrarem). **Nunca usar fotos de banco de imagens ou
fabricar/gerar fotos dela** — só a foto real, fornecida por ela, deve
ocupar esses espaços.

Para ativar uma foto: seguir os comentários no `index.html` (trocam o
`<div>` vazio por um `<div>` com `<img>`) e colocar o arquivo em
`assets/img/` com o nome esperado — ver `assets/img/README.md` para os
nomes exatos e a proporção recomendada (4:5). O CSS necessário
(`object-fit: cover`, `z-index` sobre o bloco decorativo) já existe em
`style.css`, não precisa ser adicionado.

A seção Atendimento não recebeu fotos de alimentação/estilo de vida
nesta rodada — a Maryelle marcou como opcional ("caso necessário"), e
sem uma foto real disponível a decisão foi manter os cards só com texto
em vez de usar imagem de banco genérica.

## Pendências conhecidas

- `https://seudominio.com.br/` é um placeholder nas tags `canonical` e
  `og:url` do `index.html` — trocar pelo domínio real assim que existir.
- Fotos reais da Maryelle ainda não foram adicionadas ao repositório
  (ver seção "Fotos" acima).
- Preços, endereço exato do consultório e horários não aparecem em
  nenhum lugar do site por não terem sido fornecidos — não inventar
  esses dados.

## Regras para qualquer alteração futura

1. Não reintroduzir framework, build step ou dependências de npm sem
   necessidade clara e aprovação da Maryelle — a decisão de manter o
   projeto 100% estático foi explícita.
2. Não usar verde, tons fitness/clínicos, botões em pílula ou sombras pesadas.
3. O vermelho é a cor de assinatura da marca — usar com moderação (CTAs,
   detalhes), nunca preenchendo uma seção inteira de fundo a fundo.
4. Qualquer mudança nos tokens de cor/tipografia do `style.css` precisa
   de aprovação explícita da Maryelle antes de ser aplicada.
