# 提出ファイル
- *.c
- libft.h
- Makefile

# Makefile
- addprefixしているのはいずれresourceやsrcなどとしてファイルを分割管理するため。
- [x] -MMDは依存関係のファイルリストを拡張子.dのファイルに保存してくれて、コンパイルを行ってくれるもの。
- [x] 📝 Libft のVCに潜ってきて新たに得られた知見 ranlib <libft> なるコマンドがあって、これはどうやら .d みたいに動いてくれるらしい（-MMD オプションみたいにインデックスを作成してくれる） 

# テストコマンド
nm libft.aのシンボル確認　.oもできる

# テストケース
- !strcmp （strcmp==0、つまり差なし）
- &を使う
- errno未対応
- 子プロセスとして実行することでSEGVしても動くようにする
- int と long long は確実に違う（long int と intはアーキテクチャによって同じ可能性がある）のでlong longが望ましい -- yokawada さん
	- ちなみにsigned long long でも long long int でもいける（謎知識）

# テスター
- [x] https://github.com/Tripouille/libftTester.git
- [x] https://github.com/alelievr/libft-unit-test.git
- [x] https://github.com/jtoty/Libftest.git
- [x] https://github.com/uosushi/libftester.git 自作テスター

# テスト
```
git clone <自身のlibft.git> libft
git clone https://github.com/alelievr/libft-unit-test.git
cd libft
git clone https://github.com/jtoty/Libftest.git
git clone https://github.com/Tripouille/libftTester.git
git clone https://github.com/uosushi/libftester.git
```

# リファクタ
- 1行のみのifはなるべく波括弧を消すように統一
- 1ファイルのみでの呼び出しを予定していたらstaticをつける（外部呼び出し防止）
- 自作関数を呼び出すとき、その先で使用禁止関数を使っていないか
- 全ての自作関数はstatic含めft_をつける

# コーディング規則（original）
- !value
	- 特別な理由がない限り`value == NULL`はこう書く
- chunk
	-> 本体があり、それを分割されたもの。断片や破片。
- dest
	-> 戻り値としてメモリを返す
	-> コピー先にする

# チェック項目（検査済みはx）
## Part1 libc関数（MANあり、オリジナルの挙動に従う）
- [x] is*
	- [x] 文字をASCIIコード（int型）の変数cで受け取る
	- [x] 文字cを判定する（真なら0以外、そうでなければ0を返す）
	- [x] unsigned char にしなくてよい
- [x] isalpha
	-> 英字か判定する
- [x] isdigit
	-> 数字（CHAR=0~9, ASCII=48~57）か判定する
- [x] isalnum
	-> 英数字か判定する
- [x] isascii
	-> ASCII文字（ASCII=0~127）か判定する
- [x] isprint
	-> 印字文字か判定する
- [x] strlen
	- [x] size_tでカウントする
	- [+] strnlenを追加して一括管理する -- yokawada tkomatsu さん
- [+] strnlen -- snara
	- [x] lenを上限に文字数をカウントする（コスト削減 && NULL止めなしのときのための関数）
- [x] memset
	- [x] errnoなし
- [x] bzero
	- [x] errnoなし
- [x] memcpy
	- [x] (s1 == s2)（アドレス一致）のときdstを返す
	- [x] !nのときはwhileの条件文が常にFalseなのでNULLチェック不要
	- [Guacamole] memcpy(NULL, NULL, 10) -> SEGVなし
	- [M1Mac] SEGVあり
	- [x] errnoなし
- [x] memmove
	-> 本来の挙動はSEGVなのでdstとsrcのNULLチェックはなし（alelievr/libft-unit-testだとCRASHが1個検出される）
	- [x] errnoなし
- [x] strlcpy
	-> dstsizeのNULLチェックのみ
	-> 本来の挙動はSEGVなのでdstとsrcのNULLチェックはなし（alelievr/libft-unit-testだとCRASHが1個検出される）
	- [x] errnoなし
- [x] strlcat
	- [t1] カウント上限つきのstrnlenを作る（dstsizeを上回った瞬間に終了する）
		- if (dst_len > dstsize) dst_len = dstsize;
		+ strnlen(dst, dstsize)
	- [x] 連結の余地がない（`dst_len >= dstsize`）とき、dstとsrcの長さの合計を返す
	- [x] errnoなし
- [x] toupper
- [x] tolower
- [x] strchr
	-> int型の`c`には負の値も入るため、`(unsigned char)`にキャストしchar型と比較できるようにする
	-> `c`がNULLだったときのために最後にifで処理を加える
	- [x] s, c両方ともにunsigned charに対応させて検索するが、戻り値は元の（`const char`の）アドレス
	- [x] errnoなし
- [x] strrchr
	-> Guacamoleにおいて全てOK（M1 MacだとCRASH発生）
	-> size_tに対応させたので`s[len - i]`で検索
	- [x] s, c両方ともにunsigned charに対応させて検索するが、戻り値は元の（`const char`の）アドレス
	- [x] errnoなし
- [x] calloc
	- [x] Wrap aroundするかどうかをmalloc前にチェックしてNULLを返す
	- [x] countかsizeどちらかに0が入っているとき、free用にcount*sizeを1にする
	- [x] メモリ割り当てに失敗したらerrnoを`ENOMEM`に設定
- [x] strncmp
	-> `s1[i]`および`s2[i]`を(unsigned char)でキャストし負の値を0にする
	- [t1] 上から二つのif文はいらない
	- [ ] なんならwhileの条件文に中身のifを設定して大幅に削減できそう？（必須ではないが）
	- [x] errnoなし
- [x] memchr
	- [t1] unsigned charで比較する（str）
	- [t1] unsigned charで比較する（ch）
	- [x] errnoなし
- [x] memcmp
	- [x] errnoなし
- [x] strnstr
	- [t1,t2] 実装コスト削減 cmp使うと良い 
	- [x] errnoなし
- [x] atoi
	- [t2] 真偽値は0か1なので-1も1あつかいになってしまうが、
	- [x] overflowしたときerrnoを`ERANGE`に設定

## Part2 追加関数（MANなし、CRASH防ぐ必要あり）
- [x] substr
	- [t1] カウント上限つきのstrnlenを作る（strlenはコストがかかりすぎるので）
		- if (ft_strlen(s + start) < len) len = ft_strlen(s + start);
		+ strnlen(s, len)
- [x] strjoin
	- [x] 自作strcatで連結処理
	- [t1] strcat staticつける必要あり
	- [r1] strcat 2回目のとき、`mem + ft_strlen(s1)`でアドレスをずらす
	- [t1] bzeroいらないようにする（strcatに任せる）
- [x] strtrim
	- [t1] ft_is_set は strchrと同じなのでそれ呼び出せば良い
- [x] split
- [x] itoa
	-> malloc失敗時NULLチェック
- [x] strmapi
	- [t1] malloc前に`(char *)`の方が安全
- [x] striteri
- [x] putchar_fd
	- [t1] putstr_fdに合わせfdチェック（`fd < 0`）を入れる
- [x] purstr_fd
- [x] putendl_fd
	- [t1] putstr_fdに合わせfdチェック（`fd < 0`）を入れる
	- [t1] indent前のsはputstr_fdで出力できる
- [x] putnbr_fd
	- [t1] putstr_fdに合わせfdチェック（`fd < 0`）を入れる
	- [t1] static pri
	- [ ] unsigned int、もしくはlong long
	- [x] putchar_fdを有効活用

### Bonus　42独自関数（MANなし、CRASH防ぐ必要あり）
- [x] lstnew
- [x] lstadd_front
	- [t1] lstだけじゃなくnewにもNULLチェック入れる
- [x] lstsize
- [x] lstlast
- [x] lstadd_back
	- [x] lstをNULLチェック
	- [x] 変数nodeはlstを上書きしないために必要
- [x] lstdelone
	- lst->contentをNULLにする（したほうがよい） -- snara
- [x] lstclear
	- [x] delとlstをNULLチェック（どちらか一方でもNULLなら削除できないため）
	- [x] 変数nodeはlstを上書きしないため（かつ*lstを）に必要
	- （lstdelone後に）lst->nextをNULLにする（不正な参照防止） -- snara
- [x] lstiter
	- NULLチェックは`return ;`で
- [x] lstmap
	- delのNULLチェックを外さないとcrashが発生するが、予期せぬNULL（malloc失敗など）対応のためdelのNULLチェックは必須だと考えたので、NULLチェックしておく
