# Site — Maryelle Ferreira | Nutricionista Clínica

One page estática (HTML + CSS puro, sem JavaScript, sem framework, sem
dependências de build). Feita para funcionar primeiro, ser bonita depois.

## Estrutura

```
index.html         → toda a página (Hero, Sobre, Serviços, Como funciona,
                      Atendimento, Frase de impacto, CTA final, Rodapé)
css/style.css       → todo o estilo (cores, tipografia, responsividade)
favicon.svg         → ícone da aba do navegador (monograma "MF")
assets/img/         → pasta para as fotos reais da Maryelle (ver README
                      dentro dela para instruções)
```

Não há `app/`, `components/`, `package.json` nem `node_modules` — é HTML e
CSS puros, sem etapa de build.

## Rodar localmente

Não precisa instalar nada. Duas opções:

1. **Mais simples:** dê duplo clique em `index.html` (ou clique com o botão
   direito → "Abrir com" → seu navegador).
2. **Com servidor local** (recomendado, evita qualquer restrição de
   navegador para arquivos locais):
   ```bash
   npx serve .
   ```
   e abra o endereço mostrado no terminal (geralmente
   `http://localhost:3000`).

## Publicar na Vercel

1. Suba este repositório para o GitHub (se ainda não estiver lá).
2. Na Vercel, clique em **"Add New… → Project"** e importe o repositório.
3. Em **Framework Preset**, escolha **"Other"** (site estático). Não é
   necessário configurar Build Command nem Output Directory — a Vercel
   serve os arquivos da raiz automaticamente.
4. Clique em **Deploy**.

### Antes de publicar de verdade

- Troque `https://seudominio.com.br/` (em `index.html`, tags `canonical` e
  `og:url`) pelo domínio real assim que ele existir.
- Se quiser usar um domínio próprio, configure-o nas configurações do
  projeto na Vercel após o primeiro deploy.

## Imagens

O Hero e o Sobre já usam as fotos reais da Maryelle:
`assets/img/maryelle-hero.jpg` e `assets/img/maryelle-sobre.jpg`. A
segunda foi recomprimida (de ~3,7 MB para ~150 KB, redimensionada para
1200px de largura) para manter o site rápido.

Para trocar por outra foto no futuro, basta substituir o arquivo mantendo
o mesmo nome, ou trocar o `src` do `<img>` correspondente no
`index.html`. Detalhes em `assets/img/README.md`.

## WhatsApp

Todos os botões de agendamento usam o mesmo link, definido uma única vez
em cada botão do `index.html`:

```
https://wa.me/5562994935712?text=Olá%2C%20Maryelle!%20Vim%20pelo%20seu%20site%20e%20gostaria%20de%20saber%20mais%20sobre%20as%20consultas.
```
