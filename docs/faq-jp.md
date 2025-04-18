# Pyxel よくある質問

## 新バージョンへの移行方法

<details>
<summary>バージョン1.5への移行方法</summary>

コードをバージョン 1.5 に対応させるには、以下の変更を行ってください。

- `init` の `caption` オプションを `title` にリネームする
- `init` の `scale` オプションを `display_scale` にリネームする
- `init` から `palette` オプションを削除する (初期化後に `colors` 配列でパレットカラーを変更できます)
- `init` から `fullscreen` オプションを削除する (初期化後に `fullscreen` 関数でフルスクリーンを切り替えることができます)
- キー名の未定義エラーが発生した場合、[キー定義](https://github.com/kitao/pyxel/blob/main/python/pyxel/__init__.pyi) に従ってキー名をリネームする
- `Image` クラスおよび `Tilemap` クラスの `get` と `set` をそれぞれ `pget` と `pset` に変更する
- `bltm` の `u`, `v`, `w`, `h` パラメータを 8 倍に変更する (`bltm` はピクセル単位で動作するようになりました)
- `Sound` および `Music` クラスのメンバーとメソッドを新しい名前に更新する

</details>

<details>
<summary>バージョン1.5以降で<code>pyxeleditor</code>コマンドが使えません</summary>

バージョン 1.5 以降、Pyxel のツールは`pyxel`コマンドに統合されました。リソースエディタにアクセスするには、次のコマンドを使用してください： `pyxel edit [PYXEL_RESOURCE_FILE]`

</details>

## Pyxel の学び方

<details>
<summary>Pyxelを学習するにはどこから始めればいいですか？</summary>

Pyxel のサンプルコードを 01、05、03、04、02 の順に試すのがおすすめです。

</details>

<details>
<summary>Pyxelに関する書籍はありますか？</summary>

日本語版のみですが、[公式の書籍](https://gihyo.jp/book/2025/978-4-297-14657-3)が発売されています。

</details>

## API 仕様と使い方

<details>
<summary><code>update</code>関数と<code>draw</code>関数の違いは何ですか？</summary>

`update`関数は毎フレーム呼び出されますが、`draw`関数は処理時間が許容限界を超えた場合にスキップされることがあります。Pyxel はこの設計により、レンダリング負荷や OS の割り込み処理の影響を軽減して、滑らかなアニメーションを実現しています。

</details>

<details>
<summary>PyxelのMMLのコマンドの種類と使い方を教えてください</summary>

Sound クラスの mml メソッドで使えるコマンドの種類は以下のとおりです。

- `T`(1-900)<br>
  テンポを指定する。デフォルトは 100。<br>
  `Sound.speed=900/T`の式で変換されるため、指定したテンポと誤差が出ることに注意。<br>
  テンポはサウンド全体に適用され、複数指定した場合は最後の値が使用される。
- `@`(0-3)<br>
  音色を指定する。デフォルトは 0。
- `O`(0-4)<br>
  オクターブを指定する。デフォルトは 2。
- `>`<br>
  オクターブを 1 上げる。
- `<`<br>
  オクターブを 1 下げる。
- `Q`(1-8)<br>
  クォンタイズ（発音する長さ）を指定する。8 で次の音との切れ目がなくなり、4 で半分になる。デフォルトは 7。
- `V`(0-7)<br>
  音量を指定する。デフォルトは 7。
- `X`(0-7)<br>
  音量エンベロープの定義と指定を行う。`V`の代わりに使用する上級者向けコマンド。<br>
  `X2:345`と指定すると、エンベロープ 2 番に切り替え、各音の音量を 34555...のように変化させる。音量変化の単位は 4 分音符を 8 等分した長さ。<br>
  `X2`と指定するとエンベロープ 2 番に切り替え、その番号に設定した音量エンベロープを使用する。
- `L`(1/2/4/8/16/32)<br>
  音と休符の長さを指定する。L8 が 8 分音符。デフォルトは 4
- `CDEFGAB`<br>
  指定した音階の音を再生する。<br>
  `F16`の様に後ろに 1/2/4/8/16/32 の数値を指定すると、その音だけ長さを切り替える。
- `R`<br>
  休符を再生する。<br>
  `R8`の様に休符の後ろに 1/2/4/8/16/32 の数値を指定すると、その休符だけ長さを切り替える。
- `#`または`+`<br>
  音の後ろに記述すると、その音を半音上げる。
- `-`<br>
  音の後ろに記述すると、その音を半音下げる。
- `.`<br>
  付点。音の後ろに記述すると、その音の長さを 1/2 延ばす。
- `~`<br>
  音の後ろに記述すると、その音をビブラート付きで再生する。
- `&`<br>
  次の音が同じ音程なら一つの音として繋げる（タイ）。異なる音程なら滑らかに繋げる（スラー）。

</details>

## ファイル操作とデータ管理

<details>
<summary>ファイルが読み込めません。環境が変わると失敗する時があります。</summary>

ファイルを読み込む際に、カレントディレクトリが意図したものになっているかを確認してください。<br>
Pyxel の`init`関数が呼ばれると、カレントディレクトリはそのスクリプトファイルと同じ場所に変更され、それ以降は相対パスでファイルを指定できます。しかし、`init`を呼ぶ前にファイルを開こうとした場合や`init`の後にカレントディレクトリを変更した場合には読み込みに失敗する可能性があります。

</details>

## Pyxel ツールの使い方

<details>
<summary>Pyxel Editorでパレットの色を変更できますか？</summary>

Pyxel リソースファイル（.pyxres）と同じディレクトリに、Pyxel パレットファイル（.pyxpal）を配置することで、Pyxel Editor で使用するパレットの色をリソースファイルに合わせることができます。Pyxel パレットファイルの作成方法については、README をご参照ください。

</details>

## 今後の開発計画

<details>
<summary>今後のリリースで予定している機能は？</summary>

以下の機能追加や改善を予定しています。

- Pyxel アプリランチャーの追加
- サウンド機能の刷新と MML 対応
- Pyxel Editor の操作性向上
- 子供向け Pyxel チュートリアルの追加

</details>

## ライセンスとスポンサーシップ

<details>
<summary>Pyxelを作者の許可なく商業目的で使用することはできますか？</summary>

MIT ライセンスに従い、ソースコードやライセンス表示用のファイルに著作権およびライセンスの全文を明示すれば、作者の許可を得ることなく自由に販売や配布が可能です。ただし、もし可能であれば、作者にご連絡いただいたり、スポンサーとしてご支援いただけるとありがたいです。

</details>
