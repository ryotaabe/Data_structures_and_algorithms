# Data_structures_and_algorithms

データ構造とアルゴリズムについて講義で学んだことなどをメモとして残したいと思う。

## 参考資料

本稿の作成に当たっては柴田望洋氏の新・明解 Javaで学ぶアルゴリズムとデータ構造を参照した。ド定番すぎて今更感想などを述べる必要もないかと思うが、ソフトウェアに携わるすべてのエンジニアに必要なアルゴリズムの知識について体系的に学べる書籍である是非参考にしていただきたい。

## Call by Refernce と Call by value

Call by Refernce（参照渡し） と Call by value（値渡し）は引数の渡し方を表す。以下にその例を示す。
 - 基本データ型：char,int,float,doubleなど
 - 参照型：配列、インスタンスなど

 - Call by value：基本データ型
 - Call by Refernce：参照型

Call by Refernceの例を以下に示す。

    int a[]={1};
    int b[]=a;
    b[0]=2;
    system.out.println(a[0]);

この例の場合は２が出力される。
このように、Call by valueは変数の値を直接渡すことに対して、Call by Refernceはアドレスを渡すという違いがある。
上記の例では'b[0]=2;'によって配列aのアドレスそのものが書き換えられている。

## 探索

探索とはある条件を満たすデータを探し出すことである。