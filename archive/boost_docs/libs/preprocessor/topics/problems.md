#プリプロセッサに関する既知の問題

プリプロセッサによるメタプログラミングは忌避される話題である。
これは例えば、インライン関数をマクロで定義するなどの、危険な技を伴う良くない経験が一因である。
もしプリプロセッサを使わずにすむ良い方法があるのであれば、そのようにするべきだ。

ではプリプロセッサのよく知られた問題のいくつかを、問題/解決法形式で見てみよう。

##問題 #1

プリプロセッサはスコープが無い。
したがってマクロは予期しないコードの置換をおこしてしまうことがある。

###解決法 A

全て大文字からなる識別子をマクロに使い、それらをマクロ以外では使わない。
実際この方法は予期せずコードが置換されてしまう可能性を除去する。

###解決法 B

ローカルマクロ慣用法を使う:

```cpp
#define MACRO ...
// use MACRO
#undef MACRO
```

これは特定の部分以外のコードをそのマクロによって予期せず置換されてしまうことを防ぐ。

この解決法の欠点は、#undefは自動的に付けられるわけではないため、忘れることがある、というものだ。
経験を積んだプログラマは、そのマクロを定義した(時間的に)直前または直後に `#undef` を書く。

###解決法 C

特定の接頭語をマクロに付ける。

```cpp
#define UMP_MACRO
// use UMP_MACRO
```

これは予期せぬ置換と衝突をめったに起こらなくする。
この解決法の欠点は次のようなものである:

- 名前の衝突は大きなプロジェクトでは起きる可能性がある。
- マクロは大域的な名前空間を汚染している。

*全ての解決法を可能な限り組み合わせて、スコープの問題は大抵は除去できる。*

##問題 #2

プリプロセッサコードは読みにくい。
プリプロセッサがどのようにして再帰的にマクロを展開するかを理解する必要があるし、マクロの定義を見つけたり、マクロのパラメータを頭の中で置換する必要もある。

###解決法

いかなる種類のプログラミングもそのコードがどのように実行されるか理解しなければならない。
関数やテンプレートを含むいかなるパラメータ化の技術においても、定義を見つけ、頭の中でパラメータを置換する必要がある。

しかしながら、いくつかの技を知っておくことは有益である:

- 無理のない範囲でできるだけローカルマクロを使えば、マクロの定義を探す手間が軽減する。
- コードブラウザと検索ツールはマクロ定義の発見を手助けする。
- コンパイラにプリプロセス後のソースコードを生成させてバグを探すことができる。
- プリプロセッサによるメタプログラミングを使う前に、プリプロセッサを使わない小さなバージョンを実装してみる。
	その後、それをプリプロセッサを使うものに置き換える。
	これによってコードを少しずつテストすることができる。
	経験を積んだプログラマはしばしば多くの段階を飛ばすが、もし直接書くには複雑すぎるとわかったなら、少しずつテストする方法に切替えることは常に可能である。
- もし改行となるべき場所のプリプロセッサコードに特別なシンボルを埋め込んでおけば(If you insert a special symbol into the preprocessor code in places where there should be a line break)、プリプロセス後のコードを検索と置換するツールを使って読みやすくできる。
- 特に覚えておくべき重要なことは、プリプロセッサの使用を、構造化され、理解しやすく、安全な手段に制限することである。
	構造化は複雑なシステムを理解しやすくする [[McConnell]](../bibliography.md#mcconnell)。

##問題 #3

「私はcppを廃止してしまいたい。」 - *Bjarne Stroustrup* in [[Stroustrup]](../bibliography.md#stroustrup)。

###解決法

C/C++ プリプロセッサは末永く存続する。

*実際問題としては、プリプロセッサによるメタプログラミングは決してテンプレートメタプログラミングほど単純でもポータブルでも無い [[Czarnecki]](../bibliography.md#czarnecki)。*

---

(C) Copyright [Housemarque Oy](http://www.housemarque.com) 2002

Permission to copy, use, modify, sell and distribute this document is granted provided this copyright notice appears in all copies.
This document is provided "as is" without express or implied warranty and with no claim as to its suitability for any purpose.

