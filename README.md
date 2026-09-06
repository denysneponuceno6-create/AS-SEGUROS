# AS Seguros & Consórcios — site institucional

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
| `assets/marcas/` | Onde entram os logos oficiais das companhias (veja `LEIA-ME.txt`) |

## Antes de publicar

1. **Páginas legais.** Os textos são uma base sólida, mas precisam de revisão jurídica.
2. **Ative o formulário** — passo obrigatório, descrito abaixo. Sem isso o e-mail não chega.
3. **Fotos.** A foto do Alan já está no ar em dois pontos. As imagens dos seguros ainda são de banco — trocar por fotos reais é a melhoria com maior retorno depois desta.

## Configuração

No topo do `<script>`, em `index.html`:

```js
const SITE = {
  whatsapp: '5563984565618',   // só dígitos, com DDI 55
  emails: ['alansilvaseguros@yahoo.com', 'Alansilvaseguros@icloud.com'],
  endpoint: 'https://formsubmit.co/ajax/alansilvaseguros@yahoo.com',
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

O formulário entrega a solicitação **direto nas duas caixas de entrada da empresa**, sem o visitante precisar abrir WhatsApp ou app de e-mail.

```
alansilvaseguros@yahoo.com      (destinatário)
Alansilvaseguros@icloud.com     (cópia)
```

O e-mail chega assim:

| Campo | Exemplo |
|---|---|
| Assunto | Solicitação de cotação — Seguro Auto — Maria de Souza |
| Responder para | o e-mail do próprio cliente, quando informado |
| Corpo | tabela com tipo de atendimento, nome, WhatsApp, e-mail, cidade/estado, pessoa física ou jurídica, mensagem e origem |

### ATIVE O FORMULÁRIO ANTES DE DIVULGAR O SITE

O serviço usado é o **FormSubmit** (gratuito, sem cadastro, sem chave de API). Ele exige uma confirmação única:

1. Publique o site.
2. Preencha o formulário uma vez, com dados de teste.
3. Abra a caixa de entrada do **alansilvaseguros@yahoo.com** e procure um e-mail do FormSubmit — cheque também o spam.
4. Clique em **"Activate Form"**.
5. Envie um segundo teste. Ele deve chegar em segundos nas duas caixas.

**Enquanto o passo 4 não for feito, nada é entregue.** O site continua funcionando: o visitante vê a tela de "falta só um passo" com os botões de WhatsApp e e-mail, então o contato não se perde — mas a entrega automática só começa após a ativação.

### Se algo falhar no envio

O código não deixa o lead escapar:

- espera até 8 segundos por tentativa e tenta novamente uma vez;
- verifica a resposta do serviço, não apenas se a conexão abriu;
- se ainda assim falhar, mostra "Falta só um passo para concluir" e destaca os botões de WhatsApp e e-mail com todos os dados prontos;
- o botão fica desabilitado durante o envio, evitando solicitação duplicada.

### Trocar de serviço depois

Basta mudar uma linha, `SITE.endpoint`, no topo do `<script>`:

| Situação | Valor |
|---|---|
| Atual (FormSubmit) | `https://formsubmit.co/ajax/alansilvaseguros@yahoo.com` |
| Esconder o e-mail do código-fonte | depois de ativar, o FormSubmit fornece um endereço embaralhado; troque por `https://formsubmit.co/ajax/SEU-CODIGO` |
| Formspree, Make, Zapier, RD Station | a URL que o painel do serviço fornecer |
| Desligar o envio automático | deixe `''` (o site volta a usar só WhatsApp e e-mail manual) |

Nunca coloque chaves de API ou tokens no HTML — eles ficam visíveis para qualquer visitante. Se o serviço exigir chave, ela precisa ficar no servidor.

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

## Logos das seguradoras e administradoras

O site lista as 12 seguradoras e as 4 administradoras de consórcio em cartões. Hoje cada cartão mostra o **nome** da companhia em tipografia limpa — não criei nenhum logo, porque logo recriado ou baixado de busca costuma violar o manual de marca da companhia.

Quando você conseguir os arquivos oficiais (peça o kit de parceiro a cada uma), é só:

1. salvar os PNGs em `assets/marcas/` com os nomes listados no `LEIA-ME.txt` da pasta;
2. trocar `usarLogosMarcas: false` por `true` no topo do `<script>`.

O sistema é tolerante: quem tiver arquivo aparece como logo, quem não tiver continua aparecendo pelo nome. Nenhum cartão fica vazio e nada quebra. Enquanto estiver desligado, o navegador nem tenta baixar as imagens.

O nome também fica sempre no HTML, mesmo com o logo ligado — o Google e os leitores de tela continuam lendo "Porto", "Bradesco Consórcios" e assim por diante.

## Sobre o nome

O site usa **AS Seguros & Consórcios** em todos os textos, títulos, mensagens de WhatsApp e dados estruturados.

Vale conferir dois pontos que ainda apontam para o nome antigo e não dependem do site: o domínio `ascorretoraseguros.com` e o Instagram `@ascorretoradeseguros`. Não há problema em manter — o Google entende marca e domínio como coisas separadas — mas, se você trocar algum deles, me avise que eu atualizo o canonical, o sitemap e os links.

## O que não foi inventado

Somente os dados fornecidos: nome, logo, CNPJ, Alan Silva como fundador, mais de 7 anos de experiência, endereço novo, WhatsApp, Instagram, site, as 12 seguradoras, as 4 administradoras de consórcio e as coordenadas do mapa. O CEP 77006-030 veio da placa na foto da fachada. Não há taxas, prazos, condições de financiamento, garantias de contemplação, número de clientes, prêmios, certificações ou depoimentos.

## Próximos passos sugeridos

- Foto da equipe completa, para complementar a do fundador.
- Depoimentos de clientes, com autorização por escrito.
- Páginas específicas por produto (`/seguro-auto`, `/consorcio`, `/seguro-agro`) para captar buscas de cauda longa.
- Google Search Console + Google Business Profile para "corretora de seguros em Palmas".
- Se as administradoras e seguradoras fornecerem os logos oficiais, dá para trocar os nomes por marcas — com autorização de uso.
