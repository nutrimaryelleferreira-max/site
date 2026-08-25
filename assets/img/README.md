# Fotos da Maryelle

Esta pasta é o lugar certo para colocar as fotos reais. Para que elas
apareçam no site, dois passos:

## 1. Nomes de arquivo esperados

| Seção | Nome do arquivo |
|---|---|
| Hero (topo do site) | `maryelle-hero.jpg` |
| Sobre mim | `maryelle-sobre.jpg` |

Use `.jpg` (ou troque a extensão no `index.html` se enviar `.png`/`.webp`).
Fotos com proporção próxima de **4:5** (retrato) encaixam melhor nos
espaços já reservados.

## 2. Ativar a foto no HTML

No arquivo `index.html`, cada seção tem um comentário mostrando
exatamente o que colocar. Localize:

```html
<div class="hero-visual" aria-hidden="true"></div>
```

e troque por:

```html
<div class="hero-visual">
  <img src="/assets/img/maryelle-hero.jpg" alt="Maryelle Ferreira, nutricionista clínica" />
</div>
```

O mesmo vale para `.sobre-visual`, um pouco abaixo no arquivo.

Sem essa troca manual, o site continua funcionando normalmente — os
blocos coloridos que aparecem hoje no lugar das fotos são um placeholder
de design intencional, não um erro.
