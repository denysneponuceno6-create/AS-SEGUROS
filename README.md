# AS Corretora de Seguros — site institucional

Site estático, responsivo e orientado à conversão em WhatsApp. Sem build e sem dependências externas de JavaScript: basta subir os arquivos em qualquer hospedagem (Vercel, Netlify, Cloudflare Pages, Hostinger, cPanel).

Domínio previsto: **www.ascorretoraseguros.com**

## Arquivos

| Arquivo | O que é |
|---|---|
| `index.html` | Site completo (HTML + CSS + JS em um único arquivo) |
| `politica-de-privacidade.html` | Página legal linkada no rodapé e no formulário |
| `termos-de-uso.html` | Página legal linkada no rodapé |
| `robots.txt` / `sitemap.xml` | SEO técnico, já com o domínio real |
| `assets/` | Logo, favicons, imagem de compartilhamento e foto da fachada |

## Antes de publicar

1. **Páginas legais.** Os textos são uma base sólida, mas precisam de revisão jurídica.
2. **Decida como o formulário entrega o lead** (veja a seção abaixo).
3. **Foto da equipe.** A seção "Quem Somos" usa uma imagem de banco. Uma foto real do Alan e da equipe no escritório aumenta bastante a confiança — é a troca com maior retorno.

## Configuração

No topo do `<script>`, em `index.html`:

```js
const SITE = {
  whatsapp: '5563984565618',   // só dígitos, com DDI 55
  emails: ['alansilvaseguros@yahoo.com', 'Alansilvaseguros@icloud.com'],
  endpoint: '',                // vazio = nada sai do navegador
  mensagens: { ... }           // texto de cada botão de WhatsApp
};
```

Os dois e-mails aparecem no rodapé, na seção de localização, nas páginas legais e nos dados estruturados. O botão "Enviar por e-mail" endereça a solicitação para os dois de uma vez.

### Mensagens de WhatsApp por contexto

Cada botão abre a conversa com um texto diferente, conforme o briefing:

| Onde | Mensagem |
|---|---|
| Botão flutuante, header, rodapé, CTA final | "…gostaria de falar com um especialista." |
| Hero | "…gostaria de solicitar uma cotação." |
| Seção Empresas | "…gostaria de solicitar uma proposta empresarial." |
| Seção Consórcios | "…gostaria de conhecer as opções de Consórcio." |
| Seção Financiamentos | "…gostaria de falar sobre Financiamento." |
| Envio do formulário | Monta a mensagem com o produto escolhido: "…cotação de Seguro Auto." + todos os dados preenchidos |

Os botões "Solicitar cotação" dos cards levam ao formulário já com o tipo de atendimento selecionado — assim o lead fica registrado no site antes de ir para o WhatsApp.

## Formulário: como o lead chega até você

Hoje o formulário funciona 100% no navegador. Ele valida, mostra a confirmação e oferece dois caminhos de envio:

- **Enviar dados pelo WhatsApp** — abre a conversa com (63) 98456-5618 e a mensagem já escrita.
- **Enviar por e-mail** — abre o app de e-mail do visitante com destinatário, assunto e corpo prontos, endereçado aos dois endereços da empresa.

Nos dois casos o visitante ainda precisa tocar em "enviar" no app que abriu. É o comportamento normal de site estático e funciona sem nenhum serviço contratado.

**Se você quiser que a solicitação chegue sozinha na caixa de entrada**, sem depender do app do visitante, é preciso um serviço que receba o formulário. O código já está preparado: basta preencher `SITE.endpoint`. Duas opções comuns:

| Serviço | Como funciona |
|---|---|
| FormSubmit | Sem cadastro. Aponte o endpoint para o endereço deles usando o e-mail da empresa e confirme uma vez por e-mail. |
| Formspree / Make / Zapier / RD Station | Cadastro gratuito ou pago, com painel de acompanhamento e integração com CRM. |

Me avise qual você prefere e eu deixo configurado e testado. **Nenhum dado sai do navegador enquanto `SITE.endpoint` estiver vazio** — é o padrão atual, conforme o briefing.

Para conectar a um CRM, e-mail ou backend, preencha `SITE.endpoint` com a URL que receberá um `POST` em JSON:

```json
{
  "tipo": "Seguro Auto",
  "nome": "...",
  "whatsapp": "(63) 98456-5618",
  "email": "...",
  "cidade": "Palmas / TO",
  "pessoa": "Pessoa Física",
  "mensagem": "..."
}
```

Serve qualquer destino que aceite JSON: rota própria, Formspree, Make/Zapier, RD Station, HubSpot, Google Apps Script. Nunca coloque chaves de API ou tokens no HTML — use variáveis de ambiente no servidor.

Proteções já embutidas: campo-isca invisível para robôs, bloqueio de envio em menos de 2,5 s, limite de caracteres por campo e remoção de `<` e `>` antes de reutilizar qualquer texto (anti-XSS).

## Imagens

`assets/fachada.jpg` e `.webp` vêm da foto que você enviou, recortada em 4:3 e comprimida (123 KB / 82 KB). O site entrega o WebP para quem tem suporte e cai no JPEG nos demais casos.

A logo foi recortada em quatro versões com fundo transparente, além de favicon e ícone de app:

| Arquivo | Onde é usada |
|---|---|
| `logo-as.png` | símbolo "AS" — cabeçalho |
| `logo-completo-branco.png` | lockup completo em branco — rodapé e páginas legais |
| `logo-as-branco.png` / `logo-completo.png` | reservas para outros fundos |
| `og-image.jpg` | imagem que aparece ao compartilhar o link no WhatsApp/redes |
| `originais/` | logos em alta resolução, sem compressão |

Se você tiver o arquivo vetorial da logo (SVG, AI, PDF), me envie: em vetor ela fica perfeita em qualquer tamanho.

As fotos de seguros vêm do CDN do Unsplash (licença livre para uso comercial). Para produção, o ideal é baixar, converter para WebP e hospedar junto — reduz o carregamento e elimina a dependência de terceiros.

## Decisões técnicas

- **Cores:** azul-marinho `#0A1E3A` como base, o azul da logo `#007CFD` como cor de interação e foco, cinza grafite nos blocos secundários e dourado `#C9A24B` apenas em detalhes. Textos pequenos em azul usam `#0B5FC4` para garantir contraste AA.
- **Tipografia:** Manrope nos títulos (700/800), Inter nos textos (400/500) e botões (600).
- **Performance:** zero bibliotecas, CSS e JS embutidos (uma requisição de HTML), `loading="lazy"` abaixo da dobra, `fetchpriority="high"` no hero, `aspect-ratio` em todas as imagens para não haver deslocamento de layout (CLS), mapa do Google carregado sob demanda.
- **Acessibilidade:** HTML semântico, link "pular para o conteúdo", foco visível, labels em todos os campos, `aria-invalid` nos erros, menu mobile com `aria-expanded` e fechamento por `Esc`.
- **Movimento:** entradas suaves via `IntersectionObserver`; com `prefers-reduced-motion` ativo a página fica praticamente estática.
- **SEO:** title e description do briefing, Open Graph, Twitter Cards, canonical, sitemap, robots e dados estruturados `InsuranceAgency` com endereço completo, CEP, coordenadas, fundador, CNPJ, Instagram e área de atendimento nacional.

## Sobre o nome

O briefing usa **AS Corretora de Seguros** (que também é o domínio e o Instagram), então esse é o nome que aparece no site. A razão social **AS Seguros & Consórcios**, que você me informou antes, ficou registrada no `legalName` dos dados estruturados. Se preferir que o nome comercial no site seja o novo, é uma alteração rápida — me avise.

## O que não foi inventado

Somente os dados fornecidos: nome, logo, CNPJ, Alan Silva como fundador, mais de 7 anos de experiência, endereço novo, WhatsApp, Instagram, site, as 12 seguradoras, as 4 administradoras de consórcio e as coordenadas do mapa. O CEP 77006-030 veio da placa na foto da fachada. Não há taxas, prazos, condições de financiamento, garantias de contemplação, número de clientes, prêmios, certificações ou depoimentos.

## Próximos passos sugeridos

- Foto real da equipe e do escritório por dentro.
- Depoimentos de clientes, com autorização por escrito.
- Páginas específicas por produto (`/seguro-auto`, `/consorcio`, `/seguro-agro`) para captar buscas de cauda longa.
- Google Search Console + Google Business Profile para "corretora de seguros em Palmas".
- Se as administradoras e seguradoras fornecerem os logos oficiais, dá para trocar os nomes por marcas — com autorização de uso.
