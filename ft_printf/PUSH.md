# push(); 出力順
1. (!左寄せのとき)空白埋め(length_of.padding)
2. prefix
3. (!左寄せのとき)0埋め(length_of.padding)
4. 最小桁数0埋め(length_of.precision)
5. body
6. (%fのみ)preciがDECIMAL桁数をこえたときの0埋め
7. (左寄せのとき)空白埋め(length_of.padding)

# push
## 引数
format->len : NULL文字が出現するまでの body の長さ を保証
prefix      : 符号および空白 + 0x + NULL止め
body        : 本体の文字列

次に左寄せ

length_of.prefix = prefixの長さ;
// length_ofを埋める
get_length_of(&length_of, &format)

if (parsed->done + length_of.sum >= INT_MAX)
	出力上限を超えているため、失敗とみなして`-1`を返す

if (setが空白 && 左寄せフラグが立っていない && widthがlenより1以上大きい)
	出力: 空白padding
if (prefixがNULLじゃない)
	出力: prefix
if ()
出力: ゼロpadding
出力: content
// `0`は`-`に負けるためsetのチェックが不要
if (左寄せフラグが立っている && widthがlenより1以上大きい)
	出力: 空白padding





修正
- [x] %sのpreciによる長さ制限にNULL挿入で対処しようとしたが、問題があったためpushにlenを差し込むことで制御することにした