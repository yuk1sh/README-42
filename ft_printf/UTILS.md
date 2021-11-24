parsed->done + size が INT_MAX以上になるとき push_to_parsed および 0埋め, 空白埋め関数 は 動作しない そのため、このふたつは確実に出力可能なことが保証される

utils.c

# push_to_parsed
文字列strを長さlenまで1字ずつbufferに溜め込む
総溜め込み長さ % BUFSIZ == 0 のときbufferを出力

# push_char_to_parsed
文字列strを1字だけbufferに溜め込む
総溜め込み長さ % BUFSIZ == 0 のときbufferを出力

# zero_padding
長さlenまでゼロ`0`をbufferに溜め込む
総溜め込み長さ % BUFSIZ == 0 のときbufferを出力

# space_padding
長さlenまで空白` `をbufferに溜め込む
総溜め込み長さ % BUFSIZ == 0 のときbufferを出力

<!-- # push_to_parsed
// buffer[parsed->done % BUFSIZ]は常に書き込み先を指す

# util.c
## padding_left
- [x] 右寄せ(format->left_align == 0)のとき、左に詰め物をする関数
- [x] format, content_len
- [x] contentの長さがformat->widthより小さかったら何もせず終了する
- [x] 空白と0どちらで埋めるかをフラグで切り替える
- [x] format_lenに埋めた分足しておく

## padding_right
- [x] 左寄せ(format->left_align == 1)のとき、右に詰め物をする関数
- [x] format, content_len
- [x] contentの長さがformat->widthより小さかったら何もせず終了する
- [x] 右は0埋めできないのでフラグは無視し常に空白で埋める
- [x] format_lenに埋めた分足しておく

## add_prefix
- [x] num >= 0 のときのみ実行
- [x] format.signが1なら+符号を出力
- [x] format.signが0かつformat.spaceが1なら空白を一つ出力
- [x] add_prefix1字分のスペースは呼び出す関数側で処理しておくZ -->