# 使用可能関数
malloc, free, write, va_start, va_arg, va_copy,
va_end

# Makefile
 $(NAME), all, clean, fclean and re.

# テスター
- [x] https://github.com/gavinfielder/pft.git
	1. `make`
	2. `./test`（必要であればoptionでテストケースを絞り込み）
	3. 生成される`result.txt`を読むとFAILだったケースの詳細が確認できる
- [x] https://github.com/Tripouille/printfTester.git

# 設計
- [x] 構造体でi番目の変換指定子を全て管理する
- [x] 処理の順番を整理する

# 処理の順番 (ざっくり)
## ft_printf.c
1. t_parsedと(そこに含まれる)t_formatを初期化

## parse.c
1. %が出現するまでformat全体を1字ずつ読み込んでbufferに溜め込む。
	- 溜め込んだ文字数が INT_MAX - 1 を超えたらft_printfに -1 を返して処理終了
2. %を見つけたらそれが変換指定子かを見極める。
	0. [flags][width][.precision][length][type]を全て取得する
	1. 変換指定子が成立していなかったら読み込みを巻き戻して % をそのまま出力、1 へ戻る
	2. 変換指定子が成立していたら引数を受け取るときの型を取得・指定（受け取った引数を処理して出力する、typeごとの関数(print_type.c)に渡す）

## print_type.c
1. 処理した結果を push.c の push関数 に渡す
2. push関数 から -1 が返って来たら parse.c に -1 を返す

## push.c
1. push内で出力する文字数 + これまでの溜め込んだ文字数 が INT_MAX - 1 を超えるかチェックし、超えたら ft_printf に -1 を返して処理終了
2. push内で出力する順番はPUSH.mdを参照のこと。

- [x] print_type    = 出力文字列を受け取って出力
- [x] padding       = 余白を埋める
- [x] type_to_ascii = 符号込みのascii文字列を返す 

# 構造体
## t_format	format
len				: push関数に渡されるbody文字列の出力する長さ(int)
type			: is_typeでチェックした変換指定子をchar型で保持する
left_align		: 左寄せ（右埋め）するなら1、右寄せ（左埋め）なら0
zero_padding	: デフォルトの空白の代わりに`0`で埋めるとき1
prefix_notation	: 別の形式で表示するなら1
prefix_sign		: 先頭に常に符号を付加するなら1
prefix_space	: 先頭に符号などがないとき空白を1ついれるなら1
width			: 最小幅（出力が小さければ、widthの長さになるまで埋められる）
precision		: 精度／文字列の長さ上限／最小桁数（桁が足りない時先頭0埋め）

* i番目のformatを全て管理する

## t_parsed	parsed
*buffer			: malloc(BUFSIZ)で確保された、出力を溜め込んでおくバッファ。
done			: 出力（溜め込み中も含む）の長さ（ssize_t）
format			: t_format型のデータを初期化・格納・使用して使い回す変数

* SSIZE_MAX : `9223372036854775807`
* doneは最大でも`10737418235`(INT_MAX * 5)(実際ははるかに小さい)しかない。`ssize_t`の方が8桁大きい
* そのため処理を破壊しない
* ssize_tは-1も扱える。これでステータスの代わりにもなる。

## t_push	length_of
padding			: 0や空白で埋める長さ
prefix			: 接頭子(` -+0x0X`)の長さ
body			: 本体の長さ
precision		: %fのDECIMAL_DIGITを小数点以下の長さ(精度)が超えた分の長さ
sum				: prefix + body + precision + padding の合計

* length_of は、これからpushする予定の文字列の長さを格納している。
* INT_MAXが5つ入ってもオーバーフローを起こさないため、計算の都合上push型は全てsize_tを用いている。

# Fix:
- [t1] relinkチェック用にlibft内の使わないファイルに差し込んだprintf("ABC")を間違えてpushしてしまった (invalid compile) → 消してpush（済）
- [t1] %*s, -32, "ABC"などでINT_MAXを超えた扱いとなり終了する問題
