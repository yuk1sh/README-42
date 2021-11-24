

# %f
- [f]   (0.0/0.0) : nan
- [f]  -(0.0/0.0) : -nan
- [+f]  (0.0/0.0) : +nan
- [+f] -(0.0/0.0) : -nan


# %x
- [%x] SIZE_MAX - 1 : fffffffe
- [%x] SIZE_MAX     : ffffffff
- [%x] SIZE_MAX + 1 : 0

# Mandatory part
- [x] ABCDEF\n
	- [x] 出力した字数（`\n`含む）を返す
	- [x] 与えた字を全て出力する
- [ ] %d
	- [x] format検索処理
- [ ] %+-+d
	- [x] flagsの中身はいくつ指定しても正常に動作する