# TSL a partir do Figma

Uma skill para transformar um layout do Figma em uma página de vendas com HTML, SCSS e JavaScript puro.

A ideia é simples: você envia o link da tela e a skill reúne as instruções de desenvolvimento, organização do código e revisão visual. Não precisa repetir um prompt longo a cada projeto.

## Antes de começar

Você vai precisar de:

- Um assistente com suporte a skills e acesso aos arquivos do projeto.
- A integração MCP do Figma conectada, com acesso ao arquivo que será desenvolvido e às instruções `figma-design-to-code`.
- A skill [Mayor Code Style](https://github.com/moisesmengo/mayor-code-style), do Moises, para organizar o HTML e o SCSS.
- Um link do frame no Figma, de preferência com `node-id` na URL.

Para páginas longas ou com muitos detalhes, vale usar esforço de raciocínio extra alto ou superior.

## Instalação

Se o seu assistente tiver a skill `skill-installer`, envie este pedido:

```text
$skill-installer Instale as skills destes endereços:

https://github.com/moisesmengo/mayor-code-style/tree/main/skills/mayor-code-style
https://github.com/gabrielsmaracaja/codex-skills/tree/main/skills/tsl-figma

Se a Mayor Code Style já estiver instalada, mantenha a versão existente.
```

As duas skills são instaladas separadamente. A conexão com o Figma precisa estar configurada no seu ambiente.

Para instalar manualmente, baixe este repositório e copie a pasta `skills/tsl-figma` inteira para o diretório de skills do seu assistente. Faça o mesmo com a pasta `skills/mayor-code-style` do repositório do Moises. Preserve os nomes das pastas e os arquivos que vêm dentro delas.

## Como usar

Abra a pasta em que a página deve ser criada e envie:

```text
$tsl-figma Crie uma TSL a partir deste frame: [cole o link do Figma].
```

Se houver um layout mobile separado, inclua o link também. Você pode enviar os links de checkout e outras referências na mesma mensagem.

Depois da instalação, basta chamar `tsl-figma`. Ela orienta a leitura da Mayor Code Style e das instruções do Figma durante o trabalho.

## O que já está definido

- HTML, SCSS e JavaScript puro, sem frameworks.
- HTML com poucas classes e SCSS em árvore, seguindo a Mayor Code Style.
- Textos, cores, fontes, imagens e espaçamentos baseados no Figma.
- Layout responsivo e imagens otimizadas, preservando a qualidade visual.
- Depoimentos com Swiper.
- Kits no mobile na ordem de 6, 3 e 2 frascos, com os selos de frete grátis e desconto sobrepostos às imagens quando fizerem parte da oferta.
- Comparação da página com o layout e verificação das interações antes da entrega.

A referência para os kits no mobile é [esta página](https://nerveease.info/dtc-b/). As cores, os textos e as artes continuam vindo do Figma do projeto.

## Arquivos da página

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

O `style.scss` concentra os estilos. O `style.css` é a versão compilada que o navegador carrega.

## Alguns detalhes

Sem um frame mobile, a página é adaptada a partir do desktop. Sem links de checkout, o desenvolvimento visual pode seguir, mas os destinos de compra ficam pendentes. Se o design tiver kits com outras quantidades, as ofertas reais devem ser mantidas.

A fidelidade ao layout precisa ser conferida no navegador. A skill orienta essa revisão, mas o resultado depende do acesso ao Figma, dos assets disponíveis e da execução de cada projeto.

## Créditos

A organização do código segue a [Mayor Code Style](https://github.com/moisesmengo/mayor-code-style), criada pelo Moises. Os carrosséis de depoimentos usam [Swiper](https://swiperjs.com/).
