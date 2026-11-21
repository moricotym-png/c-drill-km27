# 引き継ぎメモ（C単語ドリル）

新しい会話は前のやりとりを見られないので、続きはこのメモから始める。

## これは何か
英単語・熟語・文法を覚えるためのWebアプリ（スマホのホーム画面に置く形）。
- 公開先：https://moricotym-png.github.io/c-drill-km27/ （GitHub Pages、main の直下）
- 使う人：歯科医師の先生（Android・Chrome）と、娘さん（iPhone。Claude は使えない）
- 返答は日本語で短く、専門用語を避け、画面のどこを押すかまで書く。

## 作り
- `index.html` 1ファイル（ビルドなし）。`dict.json`（英和辞書 EJDict、パブリックドメイン）、`ipa.json`（CMU発音辞書から作った発音記号）
- 記録はスマホの localStorage（キー `c-drill-v1`）。途中の1回分は `c-drill-q`。**キー名は変えない**。
- 画面を変えたら `APP_VER` を上げる（設定の一番下に表示。開いたときに新しい版があれば自動で読みこみ直す）。
- 公開前に Playwright で実際に動かして確かめる（壊れた版を出すと、スマホが最大10分ほど覚えてしまう）。

## 単語帳・テキストの取りこみ
**単語帳やテキストの中身は、このリポジトリ（公開ページ）に入れない。** ファイルにして、先生 → LINE → 娘さん、の形で渡す。
- 先生が PDF（ScanSnap）や写真を Google ドライブに入れる → Claude が書き起こして取りこみ用 JSON を作る → ドライブに置く／ファイルで渡す
- アプリでは「追加」→「ファイルから取りこむ」、または「② 貼りつけて取りこむ」（Claudeアプリの返事・送るコード）
- 「設定」→「単語をほかのスマホに送る」で、選んだセット（単語・文法問題）を圧縮コード（`CDRILL1:`）にして LINE で送れる

### 取りこみ用 JSON の形
```json
{"set":"本の名前","prio":3,
 "words":[{"word":"put away A / put A away","meaning_ja":"Aをしまう","meaning_en":"","reading":"IPA",
           "sentence":"例文","sentence_ja":"例文の訳","syn":"類義語","ant":"反対語","note":"メモ（💡で表示）","set":"Unit117 away"}],
 "grammar":[{"set":"Unit117 文法","type":"mc","question":"Please ( ) your toys.","choices":["put away","pass away","give back","sit back"],"answer":0,"explanation":"解説","source":"p.160"},
            {"set":"…","type":"input","question":"He ____ away last year.","answer":["passed"],"explanation":"…"},
            {"set":"…","type":"order","question":"私はその本を元に戻した。","answer":"I put the book back","explanation":"…"}],
 "questions":[]}
```
- セット名は「本の名前＋空白＋set」になる。新しく覚える単語は、セット名の最初の言葉（＝単語帳）ごとに未学習の割合で割り振る。
- `prio`：同じ単語がかぶったとき、数字の大きい単語帳の中身とセットを優先（英英=3、DISTINCTION=2、パス単=1）。覚えた記録は消さない。
- 文法の `type`：mc=4択（answer は正解の番号、0から）、input=書いて答える（answer は正解の文字、複数可）、order=並べかえ（answer は正しい文。空白で区切った語を並べかえる）。
- 問題文の空欄は `____` か `( )` で書く（アプリで下線になる）。

## これまでに入れた単語帳（中身は先生のドライブ）
- パス単1級（さくいん2,100語、意味は辞書から）／DISTINCTION 1〜3（Level 1〜3、900語）／英英英単語 超上級編（857語、0801〜0843はスキャンなし）
- 娘さんのテキスト（J Institute、Unit 117 など句動詞の表・Words to Remember・文法）：これから PDF をもらって、単語と文法問題にする予定。

## 次にやること
1. 先生から娘さんのテキストの PDF（ドライブのフォルダ）をもらったら、全ページを書き起こし、単語（Unit ごとのセット）と文法問題（テキストの問題はそのまま、説明からは4択）を1つの JSON にしてドライブに置く。
2. 文法問題の形がテキストに合わなければ、アプリの文法クイズ（`renderGram` まわり）を足す。
