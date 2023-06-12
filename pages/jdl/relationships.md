---
layout: default
title: JHipsterドメイン言語 - リレーションシップ
permalink: /jdl/relationships
sitemap:
    priority: 0.5
    lastmod: 2019-11-03T12:00:00-00:00
---

# <i class="fa fa-star"></i> JHipsterドメイン言語(JDL) - リレーションシップ

## 概要

<<<<<<< HEAD
1. [リレーションシップの種類](#リレーションシップの種類)
1. [リレーションシップのメソッド](#リレーションシップのメソッド)
1. [複数のリレーションシップのボディ](#複数のリレーションシップのボディ)
1. [構文](#構文)
1. [例](#例)
   1. [基本の例](#基本の例)
   1. [注入フィールド](#注入フィールド)
   1. [結合フィールド](#結合フィールド)
   1. [メソッド](#メソッド)
   1. [必須のサイド](#必須のサイド)
   1. [再帰リレーションシップ](#再帰リレーションシップ)
   1. [コメント](#コメント)
=======
1. [Relationship types](#relationship-types)
1. [Relationship methods](#relationship-methods)
1. [Multiple relationship bodies](#multiple-relationship-bodies)
1. [Syntax](#syntax)
1. [Examples](#examples)
   1. [Basic example](#basic-example)
   1. [With injected fields](#with-injected-fields)
   1. [With joint fields](#with-joint-fields)
   1. [With methods](#with-methods)
   1. [With options](#with-options)
   1. [With required sides](#with-required-sides)
   1. [Reflexive relationships](#reflexive-relationships)
   1. [Commenting](#commenting)
>>>>>>> upstream/main

---

### リレーションシップの種類

キーワード`relationship`の後に記述されます。

次の4つのリレーションシップタイプがあります。
  - `OneToOne`
  - `OneToMany`
  - `ManyToOne`
  - `ManyToMany`

リレーションシップについてと何を達成できるかについてもっと知りたい場合は
[専用ページ](/managing-relationships)を参照ください。

名前を複数形にすることに関する注意：JHipsterはそれらを処理するので、リレーションシップの中で処理する必要はありません。

---

### リレーションシップのメソッド

元のエンティティと先のエンティティの後に記述され、`with`キーワードとともに使用されます。

サポートされているメソッドは以下です。
  - `jpaDerivedIdentifier`: `@MapsId`は、関連付けに使用されます。 (**OneToOneにのみ適用可能**)

---

### 複数のリレーションシップのボディ

JDLファイルに同じタイプの _n_ 個のリレーションシップがあることにうんざりしている場合でも、心配する必要はありません。解決策はあります。

例えば、次のJDLサンプルを見てください。
```jdl
relationship OneToOne {
  A to B
}
relationship OneToOne {
  B to C
}
relationship OneToOne {
  C to D
}
relationship OneToOne {
  D to A
}
```

解決策として、次のように、リレーションシップ宣言内にすべてのリレーションシップ本体を含めることができます。
```jdl
relationship OneToOne {
  A to B,
  B to C,
  C to D,
  D to A
}
```

この構文は、次の場合に非常に役立ちます。
  - 同じタイプのリレーションシップをたくさん持っている場合
  - どのようなリレーションシップがあるかを知りたい場合
  - JDLファイルでそれらを探すために時間を無駄にしたくない場合

---

### 構文

リレーションシップの宣言は次のように行われます。
```
relationship (OneToMany | ManyToOne | OneToOne | ManyToMany) {
  @<option>("<option value>")+ <from entity>[{<relationship name>[(<display field>)]}] to @<option>("<option value>")+ <to entity>[{<relationship name>[(<display field>)]}]+
}
```

<<<<<<< HEAD
  - `(OneToMany | ManyToOne| OneToOne | ManyToMany)`はリレーションシップのタイプです。
  - `<from entity>`は、リレーションシップのエンティティの所有者の名前です：いわゆる「元」です。
  - `<to entity>`は、リレーションシップの先となるエンティティの名前です：いわゆる「先」です。
  - `<relationship name>`は、もう一方の端をタイプとするフィールドの名前です。
  - `<display field>`は、選択ボックスに表示されるフィールドの名前です（デフォルト：`id`）。
  - `required`注入されたフィールドが必須かどうか。
  - `with jpaDerivedIdentifier`は、関連付けに`@MapsId`が使用されているかどうかを示します（1対1の場合のみ適用可能）。
  - 複数のリレーションシップ主体を持つことができます。
    - 詳細については、[複数のリレーションシップのボディ](#複数のリレーションシップのボディ)セクションを参照してください。
=======
  - `(OneToMany | ManyToOne| OneToOne | ManyToMany)` is the type of your relationship,
  - `<option>` is one of the supported values: `onDelete | onUpdate`. Make sure to put this on the correct side of the relationship.
  - `<option value>` is one of the fitting values for the given option: `NO ACTION | RESTRICT | CASCADE | SET NULL | SET DEFAULT`
  - `<from entity>` is the name of the entity owner of the relationship: the source,
  - `<to entity>` is the name of the entity where the relationship goes to: the destination,
  - `<relationship name>` is the name of the field having the other end as type,
  - `<display field>` is the name of the field that should show up in select boxes (default: `id`),
  - `required` whether the injected field is required.
  - `with jpaDerivedIdentifier` whether `@MapsId` is used for the association (applicable only for one-to-one)
  - And you can have more than one relationship body
    - See the [Multiple relationship bodies](#multiple-relationship-bodies) section for more info!
>>>>>>> upstream/main

---

### 例

#### 基本の例

```jdl
relationship OneToOne {
  A to B
}
```

この例は以下の例と同じです。
```jdl
relationship OneToOne {
  A{b} to B{a}
}
```
注入されたフィールドを指定しないことで、短い形式で双方向のリレーションシップを持たせます。

別の例です。
```jdl
relationship OneToOne {
  A{b} to B
}
```
これにより、単一方向のリレーションシップが生成されます。エンティティBはエンティティAを介してのみ探索できますが、エンティティAはエンティティBを介して探索できません。


---

#### 注入フィールド

```jdl
relationship ManyToMany {
  A{b} to B{a}
}
```

これは双方向のリレーションシップです。つまり、両方のエンティティがもう一方のエンティティの「インスタンス」を持って
生成されます。

---

---

#### メソッド

```jdl
relationship OneToOne {
  A to B with jpaDerivedIdentifier
}
```

---

<<<<<<< HEAD
#### 必須のサイド
=======
#### With options

```jdl
relationship ManyToOne {
   A to @OnDelete("SET NULL") @OnUpdate("CASCADE") B
}
```

---

#### With required sides
>>>>>>> upstream/main

少なくとも1つのリレーションシップサイドを必須にするために使用します。

```jdl
relationship ManyToMany {
  A{b required} to B{a}
}

// または

relationship ManyToMany {
  A{b} to B{a required}
}

または

relationship ManyToMany {
  A{b(name) required} to B{a required}
}
```

---

#### 再帰リレーションシップ

再帰リレーションシップは、元と先のエンティティが同じリレーションシップです。

```jdl
relationship ManyToMany {
  A{parent} to A{child}
}
```

---

#### 必須な再帰リレーションシップに関する注意

[ここ](https://github.com/jhipster/generator-jhipster/issues/11495)で指摘されているように、同じエンティティとの必須なリレーションシップ
はサポートされていません。必須とは、子は**常に**親を持たなければならず、親も常に親を持たなければならない、といったことです。
可能な回避策は、明示的なルートおよび子エンティティを持つことです。

----

#### コメント

リレーションシップにコメントを追加できます。

```jdl
relationship OneToOne {
  /** このコメントはエンティティAのbの前に置かれます*/
  A{b}
  to
  /** このコメントはエンティティBのaの前に置かれます*/
  B{a}
}
```

ここでもこれまでと同じコメント規則が適用されます。
これらのコメントは、後でJHipsterによってJavadocコメントとして追加されます。JDLには独自のコメントがあります。
  - // 無視されるコメント
  - /** 無視されるコメントではありません */

したがって、`//`で始まるものはすべてJDLの内部コメントと見なされ、Javadocとしてカウントされません。
`#`で始まるJDL Studioディレクティブは、構文解析中に無視されることに注意してください。
