# Marca Corretora de Seguros — site institucional

Site estático, sem build step e sem dependências. Basta servir a pasta.

## Estrutura

```
index.html              site completo (HTML + CSS + JS inline, vanilla)
favicon.ico             ícone da aba
vercel.json             cache de assets e headers de segurança
robots.txt / sitemap.xml
uploads/                imagens, logos das seguradoras, vídeo e poster do hero
Marca Corretora.dc.html fonte de design (Claude Design) — não é publicada
support.js              runtime do editor de design — não é publicado
```

Os dois últimos, mais backups e rascunhos, ficam de fora do deploy via `.vercelignore`.

## Rodar localmente

```bash
npx serve .
```

Um servidor com suporte a `Range` é necessário para o vídeo do hero
(`python -m http.server` não serve — ele não implementa `Range`).

## Publicar na Vercel

**Via CLI:**

```bash
npx vercel          # preview
npx vercel --prod   # produção
```

Quando perguntado pelo *Framework Preset*, escolha **Other**. Não há comando de
build nem diretório de saída: a raiz do projeto já é o que vai para o ar.

**Via Git:** faça o push da pasta para um repositório e importe em
vercel.com/new. As mesmas respostas valem.

## Depois do deploy

Trocar o domínio de exemplo `https://marcacorretora.com.br/` pelo domínio real em:

- `index.html` — `<link rel="canonical">`, as tags `og:*` / `twitter:*` e o
  bloco JSON-LD
- `robots.txt` — linha `Sitemap:`
- `sitemap.xml` — `<loc>`

## Onde mexer no conteúdo

Tudo que muda com frequência está em constantes no `<script>` do final do `index.html`:

| Constante | O que controla |
|---|---|
| `WA_PHONE` | número do WhatsApp usado pelo formulário |
| `PARTNERS` | as 17 seguradoras do carrossel (arquivo + nome para o `alt`) |
| `SMALL_SERVICES` | os 16 cards compactos atrás do botão "Outros seguros" |
| `SERVICES` | as descrições longas que abrem no modal do "Ver mais" (texto do site oficial) |
| `TESTI_VIDEOS` | os 3 depoimentos em vídeo (id do YouTube, nome, cidade e miniatura local) |
| `ICONS` | os paths SVG usados pelos cards compactos |

Cada card de seguro tem um `data-svc="<chave>"` que aponta para uma entrada de
`SERVICES`. Para adicionar uma modalidade, crie a entrada lá e use a mesma chave no card.

Os vídeos de depoimento só carregam o iframe do YouTube quando alguém clica no play; as
miniaturas ficam em `uploads/depoimento-*.jpg`.

O formulário de contato não tem backend: ele monta a mensagem e abre uma
conversa no WhatsApp com os dados já preenchidos. Nenhum dado é armazenado.
