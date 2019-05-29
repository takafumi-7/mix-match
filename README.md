# mix-match
半教師あり学習モデル「MixMatch」を論文から実装してみる(実装中)  

## 参考論文
David Berthelot, Nicholas Carlini, Ian Goodfellow, Avital Oliver, Nicolas Papernot, Colin Raffel  
MixMatch: A Holistic Approach to Semi-Supervised Learning, 2019

## 学習プロセス
1. ラベル有りデータをAugmentationする。  
Augmentation:画像で言えば、訓練データの画像を反転したもの、一部を拡大したものなどを  
新たな訓練データとして追加すること。
2. ラベル無しデータをAugmentationする。
3. 2で生成したAugmentation後の画像のラベルを学習modelで予測し、Augmentation元の画像データ一つずつの単位で平均値をとる。  
⇨予測値の平均の出力はone hot表現になっている
4. 3で生成した平均値をsharpening(エントロピーを下げる関数)にかける
5. ラベル有りデータとラベル無しデータそれぞれについてMixupする  
(Mixupの詳細は後日追記)  
6. ラベル有りデータについて、学習モデルの予測値とラベルとのクロスエントロピーを算出する  
7. ラベル無しデータについて、生成したラベルと予測値の差の二乗和を算出する。
8. 6と7を加算する（これを損失関数とする）
9. 1〜8を繰り返し、勾配降下法により8の結果が最も小さくなる重みを求める。

