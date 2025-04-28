# go 言語で作るインタプリタをつくってみた

## 新たに変更したところ

### 変数を再代入可能に変更した

let で宣言した変数を再代入可能に変更しました．
大まかな変更点は以下の通り．

- AssignExpression として再代入可能にするための構造体を追加
- parser.go 内で=を中間演算子としても使えるように登録
- evaluator.go で再代入の式を再起的に評価して，左辺の変数に右辺の値をセット

### for 文を実行可能にした

大まかな変更点は以下の通り．

- ast.go の ForStatement で初期化，継続条件，更新式，ループ本体として文字列表現を生成
- parser.go の parseForStatement 関数で for を構文解析
- evaluator.go の evalForStatement で for 文を評価

### 前置，後置インクリメントに対応

大まかな変更点は以下の通り．

- ++を，前置，後置演算子のどちらにも定義して evaluator.go の evalPreIncrementExpression，evalPostIncrementExpression で評価
- デクリメントは構文解析のみ対応．評価まではできていない．

### ファイルのインポートに対応

プログラムを記載した外部ファイルの読み込みに対応．
大まかな変更点は以下の通り．

- builtins.go に readFile 関数を組み込み，ファイルの読み込みを対応させる．
- ファイルを読み込んで文字列を構文解析したら，evaluator.go の evalExternalFile で再帰的に文を評価する
