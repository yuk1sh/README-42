# Makefile
relinkを防ぐため、make bonusではmake WITH_BONUS=1でパラメータを渡す
[Makefileにパラメータを渡す方法と条件文](https://qiita.com/liubin/items/69d9faf804e82ddec376)
$<のはたらきは、OBJSが存在しないとき順に1つずつファイル名を指定するもの。

# t_list
t_list	lst; -> [lst.content, lst-t]

# 線形リスト


# ft_lstsize
おそらくnextが次のリストへのポインタなので
tmpにtmp.nextを代入しながら次が存在しなくなるまでカウントアップ
 関数返り値がint型なので、処理がうまくいかなかった際は-1を返すなどするのも良いかも？

# lstadd_back
- lstそのものがNULLなら検索も追加もできないので`return ;`

# lstdelone
- 要素はlst
- freeしてるだけで削除してない（消すnodeを指すnextと次のnodeをつなげてあげた方が安全）@nyokota さん

# lstclear
- delのNULLチェック
- lstdeloneはこのためだけにあるのでは？ @nyokota

# lstiter
- 関数fの戻り値はvoid型なので適用後に代入しない
- NULLチェックは`return ;`で行う

# lstmap
lstclearをつかう
- NULLチェック（lst, f, del）
- splitと同じ理由で、失敗時には全てfreeする
- いちいち先頭から確認させるとコストが大きくなりかねないので、先頭のアドレスを`head`にわたしておく。
- libft-unit-testではlstmapの引数delがNULLだけどNULLを返されず処理されることを求めているみたい。中身のテストのために一度delのNULLチェックを外してから動かしたらOKが出た。new_node = ft_lstnew(f(lst->content));の時点で予期せぬNULLが返ってくる可能性を考えたらdelは必須のはずだからNULLチェックは入れたままにしておく