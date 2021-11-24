# isalpha

# isdigit

# isalnum

# isascii

# isprint
- 印字文字の範囲


# strlen
- size_tで返す

# memset
指定した1文字を

# bzero

# memcpy
- *src の先頭から n ⽂字分を、 *dst へコピーする 途中に'\0'を含んでいてもコ
ピーを続ける *dst と *src が重なっているときの動作は未定義
- s1とs2のアドレスが一致しているときはコピーが必要ないためifで処理する（dstを返す）
- ~~本来の挙動（SEGV）に合わせるためNULLプロテクトは排除~~
	->~~Guacamoleの挙動に合わせるならNULLプロテクトは必要なので差し込み~~

# memmove
- dest の先頭から len 文字分 src をコピーします。このとき、strcpy()と異なり空文字('\0')を付加することはありません。また、src を単なるメモリブロックとして扱うため、途中に空文字('\0')を含んでいてもコピーを続けます。
	- whileの条件を「空文字が出るまで」ではなく「lenが0になるまで」とする
- dest と src が重なっているときの動作は memcpy() では未定義ですが、このmemmove() では正しくコピー（つまり移動）が行われます。
	- アドレスを比較（dest < src）して前後どちらからコピーするかを考える（https://www.pllab.riec.tohoku.ac.jp/smlsharp/ja/?Tutorial%2F015）
- キャストをunsigned char * から char * と const char * へ
- len--の位置を操作後から操作前へ
- memmove(NULL, NULL, len);では`(null)`が返ってくる（挙動確認済み）

正常なftテスト
dst: CDEF
src: ABCDEF
D -> F
C -> E
B -> D
A -> C
ft : ABCD
ex : (null)
ft : (null)
処理が逆転したftテスト
test $ gcc sandbox.c ../Libft/ft_*.c
test $ ./a.out                      
memmove
dst: CDEF
src: ABCDEF
ex : ABCD
A -> C
B -> D
A -> E
B -> F
ft : ABAB
ex : (null)
ft : (null)


# strlcpy
- 文字列をdstsizeコピーするのに使う
- ft_strlen(src)でカウント済みなので、!dstsizeのときの戻り値はすぐ返せる
- `!dstsize`
	-> どちらも`len`を返しているので一見無駄に見えるが、
	-> `dst[0]`にNULLを代入してしまわないようにしているため必要
- `while (i < dstsize - 1 && src[i])`
	-> dstの長さ（dstsize）は1始まりなのでiに合わせて-1で比較する。
	-> dstの長さ（コピーできる最大値）までコピーする。
	-> ただし、`src[i]`がNULLのときはコピーできるものがないので終了する。
- 本来の挙動（SEGV）に合わせるためNULLプロテクトは排除
```
if (!src)
	return (0);
len = ft_strlen(src);
if (!dstsize)
	return (len);
```
- 検証
	- strlcpy(NULL, NULL, 10) -> SEGV
	- strlcpy(dst, NULL, 10) -> SEGV
	- strlcpy(NULL, "42Tokyo", 10) -> SEGV

# strlcat
- 文字列srcを文字列dstに連結するために使う。
- NULL止めを忘れずに
- dstsizeは（src→dstに）コピーする長さの上限を指定するために存在している
- `dst_n(dstの本来の長さ) > dstsize(コピー上限)`
	-> dstsizeは`sizeof(dst)`（dstに確保されたメモリ数）想定
	- コピー上限を超えたときは以下のようにする
	-> dst_n = dstsize
	-> `return (dst_n + src_n)`のような文を2個挟むよりよいとおもってこうした
- `dstsize - dst_len`
	-> `j + 1 < sizeof(dst) - strlen(dst)`
	-> dstのスタック内で'\0'出現以降の書き込み可能な字数かどうか
	-> jは0はじまりなので`+1`かつNULL埋め用の1バイトを考慮し`<`をつかう
	- 例
		-> dstsize = 10;
		-> dst_len = 5;
		-> 3(4) + 1 < 5 // True
		-> 4(5) + 1 < 5 // False
		-> dst[5+4] = '\0'
# toupper
isalphaと大文字変換（-32）

# tolower
toupperの逆

# strchr
- int型の c をunsigned char に変換
- c が 0 のときの挙動に未対応だったので while の後に対応するコードを配置

# strrchr
- strchrと同様の修正
- i > 0だとs[0]のときに対応できないので、i>=0に

# strncmp



# memchr
- NULL文字が出ても検索を続ける

DEBUG
- 256+iすると文字はiになるので、intからcharに変換した方が安全。

# memcmp
- '\0'での終了をなくし、*str1と*str2が一致しなくなった時点で引き算して比較を返す（正か負に振り分ける）
 - ABCD
 - ABDE
 - ならC-DしDのほうが大きいので負の値が返る。
- unsigned charで比較するところを誤ってconst charにしていたので修正。これはマイナスの値を持つint型をわたされたときの動作の違いによるエラーだった。

# strnstr
NULL以降は検索されない。
while (i < len && haystack[i] != '\0')
と
while (i < len && haystack[i])
どちらでも実装可能なはず（haystack[i]がNULLでない場合なので）

# atoi
- 符号（-+）は1つまで対応（-なら負の値として、+なら無視してi++）
- spaceを空白と解釈しis_space関数をもってこなかったらエラーが出たので修正
- LONG_MAXおよびLONG_MINをこえたときis_overが呼び出される
	-> 以下が返される
	-> (int)LONG_MAX == -1
	-> (int)LONG_MIN == 0
- `if ((num * 10 + (str[i] - '0')) / 10 != num)`
	-> ULONG_MAXを超える（`unsigned long`のnumでは扱いきれない）ときに真
- `if ((sign == 1 && num > LONG_MAX) || (sign == -1 && num > (unsigned long)LONG_MAX + 1))`
	-> LONG_MAXあるいはLONG_MINを超えていないか判定する。
	-> `unsigned long`のnumでは扱いきれる
	-> `-1`を掛け合わせたあとのnumがLONG_MINより小さいとき、atoiの本来の挙動では-1
- `const char *`には負の値も代入可能だが、unsigned charにキャストしても0になるだけなのでどちらにせよ文字0~9の範囲におさまらない。可読性を優先しキャストしない。
- **推測になる**けれど、`strtol(s, NULL, 10)`で内部処理しているためにint型で未定義の範囲まで処理されている気がする

# calloc
- ゼロ埋めしてくれるbzeroと組み合わせる。
- SIZE_Tのmaxいれても大丈夫ならOK
- mallocの`count*size`がオーバーフローしないように考慮。ゼロサムも避ける
	-> NULLチェックしてcountとsizeに1を代入することでゼロサム回避かつオーバーフロー回避
	-> NULLでもfreeできるように`malloc(1 * 1)`させる
- テストなど使ったら必ずfreeする
- グローバル変数に該当する`<errno.h>`を入れるべきかわからないので入れない
	-> 厳密にはスレッドローカルストレージとして定義されており、スレッドセーフにアクセスできる。（つまり別のスレッドでもerrnoで呼び出せるが、実態はそれぞれ異なる。他のスレッドでのエラーで上書きされない）