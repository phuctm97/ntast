# ![ntast][banner]

**N**o**t**ion **A**bstract **S**yntax **T**ree.

> by [Notion Tweet].

---

**ntast** is a specification for representing [Notion] pages in [syntax
trees][syntax-tree]. It implements the [**unist**][unist] specification. It can
represent different types of pages in Notion: Page, Table, Board, List, and
Calendar.

## Contents

- [Introduction](#introduction)
  - [Where this specification fits](#where-this-specification-fits)
  - [What this specification doesn't do](#what-this-specification-doesnt-do)
- [Nodes](#nodes)
  - [`Node`](#node)
  - [`Parent`](#parent)
  - [`Literal`](#literal)
  - [`Block`](#block)
  - [`Page`](#page)
  - [`Text`](#text)
  - [`Divider`](#divider)
  - [`ToDo`](#todo)
  - [`BulletedList`](#bulletedlist)
- [Acknowledgement](#acknowledgement)
- [License](#license)

## Introduction

This document defines a format for representing [Notion] pages as [abstract
syntax trees][syntax-tree]. This specification is written in [Typescript].

### Where this specification fits

ntast extends [unist], a format for syntax trees, to benefit from its [ecosystem
of utilities][utilities].

ntast relates to [JavaScript] in that it has a rich ecosystem of utilities for
working with compliant syntax trees in JavaScript. However, ntast is not limited
to JavaScript and can be used in other programming languages.

ntast relates to the [unified] and [unified]-based projects in that ntast syntax
trees can be used throughout their ecosystems.

### What this specification doesn't do

ntast focuses on only content. Notion-application data structures like
workspaces, users, permissions, settings, etc, aren't handled by ntast.
Ecosystem plugins can process and extend functionalities using these data from
Notion API.

## Nodes

### `Node`

```ts
interface Node extends UnistNode {
  type: NtastType;
}
```

**Node** ([**UnistNode**][dfn-unist-node]) represents a node in ntast. It
differentiates a node in ntast with nodes in other [unist]-based specifications.

### `Parent`

```ts
interface Parent extends UnistParent {
  children: Node[];
}
```

**Parent** ([**UnistParent**][dfn-unist-parent]) represents a node in ntast
containing other [nodes](#node) (said to be [children][dfn-unist-child]).

Its content is limited to only ntast nodes.

### `Literal`

```ts
interface Literal extends UnistLiteral {
  value: string;
}
```

**Literal** ([**UnistLiteral**][dfn-unist-literal]) represents a node in ntast
containing a value.

Its `value` is a `string`.

### `Block`

```ts
interface Block extends Node {
  id: UUID;
  title: InlineContent[];
}
```

**Block** ([**Node**](#node)) represents what is famously known as a "block" in
Notion. Each block has a unique `id` for references and a `title` content.

### `Page`

```ts
interface Page extends Block, Parent {
  type: "page";
}
```

**Page** ([**Block**](#block), [**Parent**](#parent)) represents a page in
Notion.

**Page** can be the [root][dfn-unist-root] of a [tree][dfn-unist-tree] or a
[child][dfn-unist-child] of another [page](#page) (also known as a subpage).

### `Text`

```ts
interface Text extends Block {
  type: "text";
}
```

**Text** ([**Block**](#block)) represents a text block in Notion. It is an
equivalence to [**MdastParagraph**][dfn-mdast-paragraph].

### `Divider`

```ts
interface Divider extends Omit<Block, "title"> {
  type: "divider";
}
```

**Divider** ([**Block**](#block)) represents a divider block in Notion. It has
no content. It is an equivalence to
[**MdastThematicBreak**][dfn-mdast-thematicbreak].

### `ToDo`

```ts
interface ToDo extends Block {
  type: "to_do";
  checked: boolean;
}
```

**ToDo** ([**Block**](#block)) represents a to-do block in Notion. Its `checked`
indicates if the to-do is checked or not.

### `BulletedList`

```ts
interface BulletedList extends Block, Parent {
  type: "bulleted_list";
}
```

**BulletedList** ([**Block**](#block), [**Parent**](#block)) represents a
bulleted list block in Notion. It may has children. It is an equivalence to
[**MdastList**][dfn-mdast-list] with `ordered = false`.

---

## Acknowledgement

ntast is created and maintained by the creator of [Notion Tweet] - a tool to
write, schedule, and automate your tweets 10x easier, directly in Notion.

Special thanks to [@wooorm](https://github.com/wooorm) for his work on [unist],
[mdast], and [unified], by which this project is heavily inspired.

## License

[CC-BY-4.0](/LICENSE) © [Minh-Phuc Tran][@phuctm97].

<!-- Definitions -->

[@phuctm97]: https://twitter.com/phuctm97
[banner]: /banner.svg
[notion tweet]: https://notiontweet.app
[notion]: https://notion.so
[syntax-tree]: https://github.com/syntax-tree/unist#syntax-tree
[unist]: https://github.com/syntax-tree/unist
[webidl]: https://heycam.github.io/webidl/
[utilities]: https://github.com/syntax-tree/unist#list-of-utilities
[javascript]: https://www.ecma-international.org/ecma-262/9.0/index.html
[typescript]: https://www.typescriptlang.org
[unified]: https://github.com/unifiedjs/unified
[mdast]: https://github.com/syntax-tree/mdast
[dfn-unist-node]: https://github.com/syntax-tree/unist#node
[dfn-unist-parent]: https://github.com/syntax-tree/unist#parent
[dfn-unist-child]: https://github.com/syntax-tree/unist#child
[dfn-unist-literal]: https://github.com/syntax-tree/unist#literal
[dfn-unist-root]: https://github.com/syntax-tree/unist#root
[dfn-unist-tree]: https://github.com/syntax-tree/unist#tree
[dfn-mdast-paragraph]: https://github.com/syntax-tree/mdast#paragraph
[dfn-mdast-thematicbreak]: https://github.com/syntax-tree/mdast#thematicbreak
[dfn-mdast-list]: https://github.com/syntax-tree/mdast#list
