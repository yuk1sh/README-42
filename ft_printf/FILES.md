cspdiuxX%

print_char.c		= c
print_string.c		= s
print_address.c		= p
print_integer.c		= di
print_unsigned.c	= u
print_hex.c			= xX
print_double.c		= f
print_percent.c		= %

# type_to_ascii
- [x] 入力
	- format
	- typeの生データ
- [x] 返されるもの
	- 符号か空白を挿入済みのint8_t型ascii文字列
	- format->len = ascii文字列の長さ


# tips
tail -c 100 log.txt
printfはINT_MAXまで出力した文字数を返し、INTMAX+1からは-1を返す

# parse.c
## parse
parse_done : inputの長さを上限とする、読み込んだ長さ
done       : 出力する(outputの)長さ。失敗したときのみ-1が返される。

失敗したときの処理
format->lenが-1ならdoneも-1で返される。

# double_to_ascii.h
uint8_t  sign 符号1bitを収納
uint16_t exp  指数部11bitを収納
uint64_t frac 仮数部52bitを収納

DECIMAL_DIGIT 1074 + 1
INTEGER_DIGIT 308 + 1

int以上とか気にせず出力しちゃう方針

# ft_putdouble.c
## double_to_idouble
- [x] integer部分が0のとき、decimal部分が0のときの処理スキップ。0でないときは処理される

## ft_init_idouble
- [x] integerとdecimalの0埋めを先にしてしまう

## frac_to_decimal
- [x] n[0] = 5;は2^(-1)(=0.5)からスタートするという意味

## frac_to_integer
- [x] while (i++ <= idouble->exp - EXP_BIAS)
	- iがexpより小さければその分2倍していく


# print_type.c