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

## Pendências conhecidas

- `https://seudominio.com.br/` é um placeholder nas tags `canonical` e
  `og:url` do `index.html` — trocar pelo domínio real assim que existir.
- Sem foto real ainda: o Hero usa um bloco decorativo em CSS
  (`.hero-visual`) no lugar da imagem. Instruções para substituir por uma
  foto real estão comentadas no `style.css` e no `README.md`.
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
