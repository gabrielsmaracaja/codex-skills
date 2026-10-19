---
name: tsl-figma
description: "Cria páginas TSL (Text Sales Letter) a partir de um frame do Figma via MCP, usando HTML, SCSS e JavaScript puro com Mayor Code Style, depoimentos em Swiper e kits responsivos. Use para implementar uma TSL ou página de vendas estática fiel ao Figma, especialmente com ofertas de 6, 3 e 2 frascos."
---

# TSL a partir do Figma

Transforme o frame informado pelo usuário em uma página de vendas completa e responsiva, preservando o conteúdo e a identidade visual do Figma. O usuário precisa invocar apenas `$tsl-figma` e fornecer o link; carregue internamente as dependências abaixo.

## Entrada e preparação

- Exija um link do Figma que identifique a tela, preferencialmente com `node-id`. Não reutilize o arquivo ou o produto de uma execução anterior. Se o link não identificar o frame, peça o link específico antes de implementar.
- Use a pasta indicada pelo usuário ou o projeto atual. Inspecione as instruções locais e os arquivos existentes antes de editar. Preserve trabalho existente; quando uma nova página não couber na estrutura atual, crie uma pasta própria com nome descritivo.
- Use os frames desktop e mobile quando fornecidos. Sem frame mobile, derive o comportamento responsivo do desktop e aplique o padrão de kits descrito abaixo. Informe essa adaptação na entrega.
- Links de checkout, fontes e referências adicionais são entradas opcionais. Aproveite os destinos já fornecidos no projeto ou no design. Se faltarem, avance na implementação visual e indique a pendência, sem inventar endereços de compra.
- Recomende **GPT-6 Astra com esforço Extra alto (`xhigh`) ou superior disponível**, conforme a preferência do usuário. A recomendação não altera o modelo da conversa: não edite configurações globais nem afirme ter trocado o modelo. Não bloqueie a execução por essa preferência. [Referência oficial do modelo](https://developers.openai.com/api/docs/models/gpt-6-astra).

## Dependências que esta skill deve carregar

1. **Mayor Code Style**: leia a skill `mayor-code-style` instalada antes de escrever HTML/SCSS. Use o caminho do catálogo de skills da sessão; em uma instalação com pastas irmãs, o arquivo é [../mayor-code-style/SKILL.md](../mayor-code-style/SKILL.md). Se não estiver no catálogo, procure nos diretórios de skills pessoais e do projeto. Aplique a versão instalada, sem substituí-la por um resumo desta skill. Se estiver ausente, indique a instalação a partir do [repositório da Mayor Code Style](https://github.com/moisesmengo/mayor-code-style/tree/main/skills/mayor-code-style).
2. **Figma design to code**: leia `figma:figma-design-to-code` (ou `figma-design-to-code`, conforme o catálogo) antes de chamar `get_design_context`. Siga as instruções vigentes dessa skill e dos tools para screenshots, contexto detalhado e download de assets. Não grave caminhos de cache com versões fixas como dependências do projeto.
3. **MCP do Figma**: use a conexão disponível e o acesso ao arquivo informado. Se a conexão ou o acesso falhar, informe o impedimento específico; não invente o layout para aparentar fidelidade.

As instruções específicas do usuário prevalecem. O Figma define a aparência e o conteúdo; Mayor Code Style define a organização do código. Seus exemplos de seções não autorizam adicionar, remover ou reordenar conteúdo da tela de referência. As regras de kits mobile abaixo são a adaptação padrão solicitada para este fluxo.

## Ler o design completo

1. Extraia `fileKey` e `nodeId` do link, normalizando um ID como `20048-1842` para `20048:1842`.
2. Obtenha `get_design_context` com screenshot e os parâmetros exigidos pela skill do Figma, incluindo `skillNames`. Se o screenshot não vier, obtenha-o com `get_screenshot` antes de implementar.
3. Se a resposta for esparsa, truncada ou não cobrir toda a página, identifique os nós filhos e obtenha o contexto detalhado das seções faltantes. Relacione os nós à captura e consulte filhos independentes em lote quando possível. Uma TSL longa exige leitura até o rodapé.
4. Identifique a ordem das seções, textos completos, tipografia, pesos, medidas, cores, fundos, bordas, imagens, ofertas e interações. Mapeie cada asset ao nó, seção e posição correspondentes.
5. Reutilize recursos locais que sejam correspondências exatas. Baixe os demais assets pelo procedimento indicado nas respostas do Figma, salvando as imagens usadas em `assets/img/`. Não deixe URLs temporárias do Figma na página.

Implemente a partir do contexto detalhado. Use as capturas como alvo de comparação visual, nunca como imagem de fundo que substitui o HTML da tela. Preserve texto como texto, links como links e controles como elementos interativos.

## Estrutura e tecnologias

Use exclusivamente **HTML, SCSS e JavaScript puro**. Não use React, Next, Vue, Tailwind ou outro framework. Traduza eventuais exemplos gerados pelo Figma para essa stack.

```text
index.html
assets/
├── img/
├── css/
│   ├── style.scss
│   └── style.css
└── js/
    └── main.js
```

- O arquivo inicial é `index.html`. Toda a estilização autoral fica em `assets/css/style.scss`, e o comportamento autoral em `assets/js/main.js`.
- Compile o SCSS para `assets/css/style.css` e referencie o CSS no HTML. Entregue os dois sincronizados; o navegador não interpreta SCSS diretamente.
- Preserve as três pastas principais de `assets`. Se fontes locais forem necessárias, use uma subpasta de `assets/css/`. Arquivos locais do Swiper podem ficar em subpastas de `assets/css/` e `assets/js/`.
- Use a compilação existente ou Sass diretamente. Não introduza bundler, framework ou cadeia de build para esta página estática.
- Carregue `main.js` com `defer` e respeite a ordem das dependências.

## Organização com Mayor Code Style

Aplique a skill completa, com atenção a estas decisões para a TSL:

- HTML limpo, poucas classes e nomes locais simples. Use classes para seções, componentes reutilizáveis e elementos que precisem ser identificados; evite wrappers sem função.
- SCSS em árvore, escopado pela seção. Não use BEM nem nomes com `__` nas classes autorais.
- Mantenha as media queries dentro dos blocos que alteram e as seções do SCSS na mesma ordem do HTML. Use o breakpoint principal da Mayor Code Style e ajuste quando o conteúdo ou o frame mobile exigir.
- Siga a organização de `@font-face`, variáveis, mixins, reset/base, componentes/utilitários e seções definida pela skill.
- Respeite a convenção da Mayor Code Style para `<main>` e as exceções já previstas nela para projetos existentes.
- Preserve as classes exigidas pelo Swiper. Elas são parte da API da biblioteca e não devem ser removidas para reduzir a quantidade de classes.
- Prefira grid/flex e conteúdo no fluxo normal. Reserve posicionamento absoluto para sobreposições locais do design, como os badges dos kits.

## Fidelidade, imagens e responsividade

- Reproduza todas as seções do frame: textos, preços, moedas, descontos, depoimentos, imagens, fontes, espaçamentos e proporções. Não crie novas promessas, números ou depoimentos para preencher lacunas.
- Não redesenhe nem substitua logos, frascos, selos ou ilustrações fornecidas pelo Figma. Preserve o enquadramento e a composição dos assets.
- Otimize o peso das imagens sem perda visual significativa, mantendo transparência e legibilidade dos rótulos. Preserve os originais e compare qualquer derivado otimizado antes de usá-lo; não converta SVG em bitmap apenas para uniformizar formatos.
- Preserve proporções e dimensões adequadas. Não force largura e altura que distorçam imagens ou sobrescrevam indevidamente as dimensões intrínsecas de SVGs.
- Defina dimensões ou proporção para reservar espaço das imagens. Use carregamento tardio abaixo da primeira dobra; não aplique lazy loading à imagem principal visível no carregamento inicial.
- Adapte o layout de forma fluida e sem rolagem horizontal. Não comprima uma página desktop inteira com `transform: scale()` e não esconda seções para fazê-las caber.
- Implemente as interações presentes no design, como FAQ, navegação e CTAs. Preserve foco visível, operação por teclado e nomes acessíveis.

## Depoimentos com Swiper

- Quando houver seção de depoimentos, use **Swiper**, na versão para JavaScript puro. Consulte a [documentação oficial](https://swiperjs.com/get-started) para a versão e a API que utilizar.
- Use CSS e JS da mesma versão e fixe a versão exata; evite `latest`. Reutilize uma instalação local compatível quando existir.
- Siga o padrão de markup da Mayor Code Style: `.testimonials`, `.swiper`, `.swiper-wrapper` e `.swiper-slide`.
- Configure quantidade de slides, espaçamento, paginação e setas conforme o Figma e a largura disponível. Mantenha o gesto de deslizar no mobile e os controles acessíveis.
- Inicialize em `main.js`, apenas quando o elemento existir. Ajuste o comportamento ao número real de depoimentos; não force `loop` se não houver slides suficientes. Não invente autoplay.

## Kits no mobile

A referência padrão para a composição mobile é [nerveease.info/dtc-b/](https://nerveease.info/dtc-b/). Inspecione-a em largura de celular durante a implementação, além de qualquer imagem anexada pelo usuário. Extraia o arranjo dos cards, frascos, preços e botão; use o Figma da tarefa para cores, tipografia, conteúdo e artes. Não copie preços, checkout ou identidade do produto da referência.

- Exiba os kits em ordem **6, 3 e 2 frascos** no mobile. No desktop, mantenha a ordem do Figma. Aplique a regra a todas as ocorrências da seção de kits na TSL.
- Organize a composição interna dos cards conforme a referência mobile observada. Use a mesma estrutura de conteúdo nas duas versões sempre que viável, preservando leitura e navegação por teclado coerentes.
- Sobreponha o badge **“Free Shipping”** e o **vetor vermelho com o valor do desconto** à imagem dos frascos, nos kits aos quais pertencem. Mantenha a arte, o texto e a posição relativa especificados no Figma; não invente frete grátis para uma oferta que não o tenha.
- Use um contêiner local com `position: relative` para frascos e badges, sobreposições com `position: absolute` e uma ordem de camadas explícita. Garanta que os selos não sejam cortados, não cubram informações essenciais e não causem overflow.
- Use os assets originais dos selos. Se o Figma separar o vetor e o texto, reproduza essa composição; se o texto já estiver incorporado à arte, não o duplique.
- Se o design tiver outras quantidades de frascos, preserve as ofertas reais e esclareça a ordem necessária; não crie kits fictícios de 6, 3 ou 2.
- Se a referência externa estiver indisponível e não houver imagem anexada, continue com o Figma e as regras explícitas acima, informando que a composição específica da referência não pôde ser conferida. Não afirme ter visto um anexo ausente.

## Verificação e entrega

Antes de entregar:

1. Compile o Sass e confira os caminhos de CSS, JS, fontes e imagens. Verifique que os arquivos de assets usados existem e não estão vazios.
2. Abra a página em um navegador com servidor local. Compare todas as seções com o Figma na largura original do frame e revise também em tablet e celular, incluindo larguras como 390 px e 320 px.
3. Confira cada asset em sua posição, camada e proporção renderizada. Revise especialmente frascos, selos, fontes, quebras de texto, preços e espaçamentos. Corrija divergências visíveis dentro do escopo solicitado.
4. Teste o Swiper, FAQ, âncoras e destinos dos CTAs. Confira a ordem 6/3/2 e a sobreposição dos badges no mobile, sem efetuar compras. Investigue erros de console e recursos que não carregam.
5. Confira HTML com classes autorais enxutas, SCSS em árvore, media queries locais e ausência de frameworks ou URLs temporárias do Figma.

Entregue os arquivos e uma prévia local quando disponível. Resuma o que foi implementado, as larguras e interações verificadas e qualquer pendência real, como um checkout ou fonte não fornecido. Se alguma verificação não puder ser feita, diga qual; não declare “100% fiel” sem uma comparação visual que sustente a afirmação.

## Exemplo de chamada

```text
$tsl-figma Crie uma TSL neste projeto a partir deste frame: [link do Figma com node-id].
```

O link é variável em cada execução. A chamada já inclui o fluxo de leitura do Figma, Mayor Code Style, estrutura de pastas, responsividade, imagens, Swiper e kits mobile, sem precisar repetir o prompt original.
