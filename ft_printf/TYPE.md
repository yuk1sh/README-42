0123456

# push
- [0] widthによる空白埋め (length_of.padding)
- [1] prefix の出力
- [2] widthによるゼロ埋め (length_of.padding)
- [3] precisionによる最小桁数までのゼロ埋め (length_of.precision) 
- [4] body の出力
- [5] precisionによるDECIMAL_DIGITを超えた分のゼロ埋め (length_of.precision) 
- [6] widthによる空白埋め (length_of.padding)

# c
# s
# p  [012]

# di [01234 6]

テストケース
- [10.5] `     00001`

# f  [012 456]