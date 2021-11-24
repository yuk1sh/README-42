# define
DECIMAL_DIGIT : 少数部分の(最大)桁数
INTEGER_DIGIT : 整数部分の(最大)桁数

EXP_BIAS      : 指数バイアス
EXP_MAX       : 指数は11bitなので、表せる数は2048通りとなる。0から数えるため、指数の最大は2047となる。

FRAC_BIT      : 仮数部のbit桁数。単精度は23bit, 倍精度は52bit
EXP_BIT       : 指数部のbit桁数。単精度は8bit,  倍精度は11bit

# functions
## print_double
1. char型の配列[整数部分の(最大)桁数 + 小数点(1) + 少数部分の(最大)桁数 + NULL止め(1)]
2. 

## double_to_idouble



- [ ] paddingの出力
	1. 右寄せのときのpadding
	2. 埋めた回数をformat->lenに追加
	3. idouble本体の出力
	4. 左寄せのときのpadding
	5. 埋めた回数をformat->lenに追加
	- [ ] padding実行時点で(format->len == format->width)が決定する
- [ ] idouble本体の出力
	1. integerの余白を削る(先頭から)
	2. prefix(` `か`+`フラグの付与)
	3. integer部分の出力
	4. decimalの余白を削る(末尾から)
	5. decimal部分の出力
- [ ] idouble本体の出力 - その2
	- [ ] 先頭からintegerの余白を削る
	- [ ] もしDECIMAL_DIGITより精度(precision)が小さかったら
		- 末尾からdecimalの余白を削ることでdecimalの本当の長さを取得する
	- [ ] integerの長さ + decimalの長さ < width ならpaddingされる
	- [ ] 出力
		0. padding_left
		1. prefix(` `か`+`フラグの付与)
		2. integer部分の出力
		3. ドット(.)出力
		4. decimal部分の出力 
		5. padding_right