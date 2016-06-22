# Guia de estilo CSS / Sass Airbnb

*Uma abordagem mais razoável para CSS e Sass*

## Tabela de conteúdo

  1. [Terminologia](#terminologia)
    - [Regras](#regras)
    - [Seletores](#seletores)
    - [Propriedades](#propriedades)
  1. [CSS](#css)
    - [Formato](#formato)
    - [Comentários](#comentarios)
    - [OOCSS e BEM](#oocss-e-bem)
    - [Seletores ID](#seletores-id)
    - [JavaScript hooks](#javascript-hooks)
    - [Border](#border)
  1. [Sass](#sass)
    - [Sintaxe](#sintaxe)
    - [Ordenação](#ordenacao-de-declaracao-de-propriedades)
    - [Variáveis](#variaveis)
    - [Mixins](#mixins)
    - [Extend](#extend)
    - [Seletores agrupados](#seletores-agrupados)
  1. [Tradução](#traducao)

## Terminologia

### Regras

Uma “declaração de regra” é o nome dado ao seletor (ou grupo de seletores) acompanhados de um grupo de propriedades. Segue um exemplo:

```css
.listing {
  font-size: 18px;
  line-height: 1.2;
}
```

### Seletores

Em uma declaraçao de regra, "seletores" são os bits que determinam quais elementos na árvore de DOM serão estilizados pelas propriedades definidas. Seletores podem coincidir com elementos HTML, assim como classes, ID ou qualquer um de seus atributos. Seguem exemplos de seletores:

```css
.my-element-class {
  /* ... */
}

[aria-hidden] {
  /* ... */
}
```

### Propriedades

Finalmente, propriedades são os elementos selecionados de uma regra de declaração. Propriedades são pares de chave-valor, onde uma regra de declaração pode conter uma ou mais declarações de propriedades. Declaração de propriedades são mostradas a seguir:

```css
/* some selector */ {
  background: #f1f1f1;
  color: #333;
}
```

## CSS

### Formato

* Use "soft tabs" (2 espaços) para identação.
* Prefira dashes `(-)` no lugar de camelCasing em nomes de classes.
  - Underscores `(_)` e PascalCasing podem ser utilizados caso você use BEM (veja [OOCSS e BEM](#oocss-e-bem) abaixo).
* Não use seletores ID.
* Quando usar múltiplos seletores em uma regra de declaração, ponha cada um em uma própria linha.
* Coloque um espaço antes da abertura de chaves `{` em declaração de regras.
* Em propriedades, coloque um espaço depois, mas não antes do caractere `:` (dois-pontos).
* Coloque chave de fechamento `}` de uma regra de declaração em uma nova linha.
* Coloque linhas em branco entre declarações de regra.

**Ruim**

```css
.avatar{
    border-radius:50%;
    border:2px solid white; }
.no, .nope, .not_good {
    // ...
}
#lol-no {
  // ...
}
```

**Bom**

```css
.avatar {
  border-radius: 50%;
  border: 2px solid white;
}

.one,
.selector,
.per-line {
  // ...
}
```

### Comentários

* Prefira comentários de linha (`//` em Sass) para blocos de comentários.
* Prefira comentários em uma única linha. Evite comentários no final da linha.
* Escreva comentários detalhados para códigos que não são auto-documentados:
  - Usos do z-index
  - Compatibilidade ou hacks específicos de navegadores

### OOCSS e BEM

Nós incentivamos algumas combinações de OOCSS e BEM por três razões:

  * Ajuda a criar relações claras e estritas entre CSS e HTML.
  * Nos ajuda a criar componentes reutilizáveis e que podem ser repostos.
  * Permite menor especificidade e agrupamento.
  * Ajuda na construção de estilos escaláveis.

**OOCSS**, ou “Object Oriented CSS”, (CSS orientado a objetos) é uma abordagem para escrita de CSS que encoraja você a pensar sobre seus estilos como uma coleçao de "objetos": reutilizáveis, trechos repetitivos que podem ser usados de forma independente em todo o website.

  * [OOCSS wiki](https://github.com/stubbornella/oocss/wiki) de Nicole Sullivan
  * [Introduction to OOCSS](http://www.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/) de Smashing Magazine

**BEM**, ou “Block-Element-Modifier”, é uma _convenção de nomes_ para classes em HTML e CSS. Foi originalmente desenvolvido por Yandex com largas bases de códigos e escalabilidade em mente, e pode servir como um sólido conjunto de orientações para implementação OOCSS.

  * [BEM 101](https://css-tricks.com/bem-101/) de CSS Trick
  * [introduction to BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/) de Harry Roberts

Nós recomendamos um variante do BEM com "blocos" em formato PascalCased, que funciona particularmente bem quando combinado com componentes (por exemplo React). Underscores e dashes ainda são utilizados para modificadores e filhos.

**Exemplo**

```jsx
// ListingCard.jsx
function ListingCard() {
  return (
    <article class="ListingCard ListingCard--featured">

      <h1 class="ListingCard__title">Adorable 2BR in the sunny Mission</h1>

      <div class="ListingCard__content">
        <p>Vestibulum id ligula porta felis euismod semper.</p>
      </div>

    </article>
  );
}
```

```css
/* ListingCard.css */
.ListingCard { }
.ListingCard--featured { }
.ListingCard__title { }
.ListingCard__content { }
```

  * `.ListingCard` é um “bloco” e representa um componente de alto nível
  * `.ListingCard__title` é um “elemento” e representa um decendente de `.ListingCard` que ajuda a compor o bloco como inteiro.
  * `.ListingCard--featured` é um "modificador" e representa um estado diferente ou variação no block `.ListingCard`.

### Seletores ID

Enquanto é possível selecionar elementos por ID em CSS, isto deveria ser considerado um anti-projeto. Seletores ID introduzem um nível alto de [especificidade](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) para suas declarações de regras e eles não são reutilizáveis.

Para mais detalhes leia o seguinte [artigo do CSS Wizardry](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) sobre como ligar com especificidade.

### JavaScript hooks

Evite vincular a mesma classe em seu CSS e JavaScript. Combinando os dois, muitas vezes resulta em tempo perdido durante refatoração, quando um desenvolvedor deve fazer uma referência cruzada de cada classe que está alterando e, pior ainda, o medo de quebrar alguma funcionalidade.

Recomendamos a criação de classes específicas JavaScript para vinculação, com prefixo `.js-`:

```html
<button class="btn btn-primary js-request-to-book">Request to Book</button>
```

### Border

Utilizar `0` ao invés de `none` para especificar que um estilo não tem borda.

**Ruim**

```css
.foo {
  border: none;
}
```

**Bom**

```css
.foo {
  border: 0;
}
```

## Sass

### Sintaxe

* Use sintaxe `.scss`, nunca a original `.sass`
* Ordene as declarações e CSS regular em `@include` logicamente (veja abaixo)

### Ordenação de declarações de propriedades

1. Declaração de propriedades

    Listar todas as declarações de propriedades padrão, tudo que não seja  `@include` ou um seletor agrupado.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      // ...
    }
    ```

2. Declarações `@include`

    Agrupar `@include`s no final torna mais fácil a leitura do seletor inteiro.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);
      // ...
    }
    ```

3. Seletores agrupados

    Seletores agrupados, _se necessário_, vão no final e nada vai depois dos mesmo. Adicionar espaços em branco entre suas regras de declaração e seletores agrupados, bem como entre seletores agrupados adjacentes. Aplicar as mesmas regras acima para seus seletores agrupados.

    ```scss
    .btn {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);

      .icon {
        margin-right: 10px;
      }
    }
    ```

### Variáveis

Prefira nomes de variáveis com dash-cased (ex. `$my-variable`) do que camelCased ou snake_cased. É aceitável adicionar prefíxos em nomes de variáveis que se destinam a ser usadas dentro do mesmo arquivo com um "underscore" (ex. `$_my-variable`).

### Mixins

Mixins devem ser usados para limpar seu código, adicionar clareza ou abstrair complexidade, assim como funções bem nomeadas. Mixins que aceitam nenhum argumento podem ser úteis para isto, mas note que se você não compactar seu payload (ex. gzip) isto pode contribuir para duplicação de código desnecessário nos estilos resultantes.

### Extend

`@extend` deve ser evitado porque possui comportamento não intuitivo e perigoso, especialmente quando usado em seletores agrupados. Mesmo extendendo os seletores placeholder de nível superior, isto pode causar problemas se a ordem dos seletores mudar mais tarde (ex. se eles estão em outros arquivos e a ordem dos mesmos variar). Gzipping lida com a maioria das economias que você teria ganho usando `@extend`, e, ainda, você pode limpar seus estilos muito bem com mixins.

### Seletores agrupados

**Não agrupe seletores para mais de três níveis de profundidade!**

```scss
.page-container {
  .content {
    .profile {
      // STOP!
    }
  }
}
```

Quando os seletores se tornam muito longos, você provavelmente está escrevendo CSS como:

* Fortemente aclopado ao HTML (frágil) *—OU—*
* Excessivamente específico (poderoso) *—OU—*
* Não reutilizável


Novamente: **nunca agrupe seletores ID!**

Se você deve precisar utilizar um seletor ID (e você realmente não deveria fazer isto!), eles nunca devem ser agrupados. Se você está fazendo isto, revise sua marcação ou reflita o porquê tanta especificidade é necessária. Se você está escrevendo HTML e CSS bem formados, você **nunca** deveria usar isto!

## Tradução

  Este guia também está disponível em outras linguas:

  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese (Simplified)**: [Zhangjd/css-style-guide](https://github.com/Zhangjd/css-style-guide)
  - ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**: [Nekorsis/css-style-guide](https://github.com/Nekorsis/css-style-guide)
  - ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [nao215/css-style-guide](https://github.com/nao215/css-style-guide)
  - ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [ismamz/guia-de-estilo-css](https://github.com/ismamz/guia-de-estilo-css)
