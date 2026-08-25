# Site — Maryelle Ferreira, Nutricionista Clínica

Site institucional em Next.js (App Router) + Tailwind CSS v4, direção
visual "Terracota Editorial" (paleta Vermelho Seco). Documentação
completa de decisões de design, conteúdo e auditorias em [`CLAUDE.md`](./CLAUDE.md).

## Rodando localmente

```bash
npm install
npm run dev
```

Abra [http://localhost:3000](http://localhost:3000).

## Build de produção

```bash
npm run build
npm run start
```

## Antes de publicar

1. **Domínio real:** `lib/seo.ts` usa `https://seudominio.com.br` como
   placeholder. Troque pelo domínio definitivo (ou defina a variável de
   ambiente `NEXT_PUBLIC_SITE_URL`) — afeta canonical, sitemap.xml,
   robots.txt e Open Graph.
2. **Contato:** a página `/contato` ainda não tem e-mail nem endereço do
   consultório (não foram fornecidos). O WhatsApp já está configurado e
   funcional em `lib/whatsapp.ts`.

## Estrutura

```
app/            → páginas (App Router) — Home, Sobre, Consultas,
                  Como funciona, FAQ, Contato
components/
  sections/     → seções de página (Hero, Navbar, Footer, etc.)
  ui/           → componentes base (Button, Card, Input, WhatsAppButton...)
  seo/          → dados estruturados (JSON-LD)
lib/            → configuração central (WhatsApp, SEO, dados do FAQ)
public/images/  → fotos reais usadas no site
public/fonts/   → Cormorant Garamond e Manrope autohospedadas
```

## Stack

Next.js 16 · TypeScript · Tailwind CSS v4 · componentes no padrão
shadcn/ui (implementados manualmente, sem CLI — ver nota em `CLAUDE.md`).
