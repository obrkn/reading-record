# 安全なWebアプリケーションの作り方
XSS対策 P137<br>
・HTML要素はエスケープ<br>
・属性値はエスケープ<br>
・文字エンコードを明示　content-type text/html; charset=UTF-8<br>
（保険）<br>
・X-XSS-Protectionレスポンスへっだ<br>
・入力値の検証<br>
・クッキーにHttpOnly属性付与<br>
・TRACEメソッド無効化<br>
<br>
SQLインジェクション対策 P162<br>
・静的プレースホルダを使う（動的プレースホルダはアプリ側なので避けることが望ましい）<br>
<br>
CSRFとXSSの比較 P181<br>
<br>
CSRF対策 P196<br>
意図したリクエストか？３種 P192<br>
・秘密情報（トークン）の埋め込み railsはデフォ？　暗号論的擬似乱数生成器 P192<br>
セッションidをhiddenパラメータに埋める<br>
・パスワード再入力 P194<br>
amazonみたいな物品購入の直前、共有PCでログアウトしてるかわからない<br>
・Refererのチェック<br>
注意点としてファイアーウォールやブラウザの設定でRefererを送信しないユーザーがいる、チェック漏れもある、社内システム用。<br>
（保険）重要な処理の後にメールを送る<br>
<br>
クリックじゃっキング P202<br>
・X-Frame-Options: DENY　を指定し、frameやiframeを使用不可にする。nginxで指定してしまって良い。add_header X-Frame-Options SAMEORIGIN;<br>
<br>
セッションハイジャック<br>
一覧 P205　まとめ P230<br>
・セッションIDの推測 P209<br>
ライブラリを使う、無いなら暗号論的擬似乱数生成器を使う<br>
・URL埋め込みセッションID<br>
URL埋め込みで掲示板等に罠サイトに誘導され、Refererから取得される<br>
・セッションIDの固定化 P227<br>
認証後にセッションIDを変更する or セッションIDが変更できない場合はトークンにより対策する<br>
<br>
オープンリダイレクト P236<br>
以下のいずれか<br>
・リダイレクト先のURLを固定にする<br>
・リダイレクト先のURLを直接指定せず番号指定にする<br>
・リダイレクト先のドメイン名をチェックする<br>
（保険）クッションページ<br>
<br>
HTTPヘッダ・インジェクション P250<br>
・外部からのパラメータをHTTPレスポンスヘッダとして出力しない<br>
・リダイレクト（Locationヘッダー）やクッキー生成を専用APIに任せる<br>
・ヘッダ生成するパラメータの改行文字をチェックする<br>
<br>
セッションとクッキーの違い P255<br>
https://hara-chan.com/it/programming/session-cookie-difference/<br>
・クッキーはクライアントのブラウザに保存、寿命がない<br>
・セッションはサーバーに保存、寿命がセッション限り<br>
<br>
クッキー出力にまつわる脆弱性<br>
セキュア属性をOFFにすればHTTPでもクッキーを送れるが、罠サイトからリクエストを送るときにクッキーが送られるので、盗聴される危険性がある<br>
・セッションIDのクッキーにセキュア属性をつける OR トークンを利用する P262<br>
<br>
クッキーの属性値説明一覧 P266<br>
<br>
【認証】<br>
・SQLインジェクション<br>
・パスワード試行 ブルートフォース攻撃など<br>
・ソーシャルエンジニアリング　ショルダーハックなど<br>
・フィッシング<br>
<br>
辞書攻撃、ジョーアカウント攻撃、ブルートフォース攻撃、リバースブルートフォース攻撃、パスワードスプレー攻撃、パスワードリスト攻撃<br>
パスワードの条件 P461<br>
・8文字以上、ポリシーチェック<br>
アカウントロック P465<br>
・失敗回数を数える、10回したらロック、ログインしたらリセット、30分で解除<br>
パスワードを狙った攻撃への対策 P471<br>
・二段階認証、積極的なパスワードチェック、ログイン失敗率の監視<br>
<br>
パスワードの保存<br>
SQLインジェクションによるデータベース漏洩時の対策<br>
・秘密鍵による暗号化・複合　→ 安全な保管と頻繁に参照が必要　→ メッセージダイジェスト<br>
・メッセージダイジェスト　一方向性（復元不可）、衝突耐性（ユニーク） P473<br>
→しかし、パスワード解析方法がある<br>
・オフラインブルートフォース攻撃　8桁の暗号ならMD5で約14分、SHA-1で約39分<br>
・レインボーテーブル　あらかじめ１欄を作成（8文字〜10文字）<br>
<br>
解決方法 P477<br>
・ソルト・・・レインボーテーブル対策。20文字以上、ユーザーごとに異なる。<br>
・ストレッチ・・・オフラインブルートフォース攻撃対策。ハッシュ関数を繰り返す、「遅い」ハッシュ関数であるBCrypt、PBKDF2、Argon2の使用。<br>
<br>
自動ログイン<br>
・セッションの寿命を延ばす・・・セッションのExpires属性を1週間に延ばし、ユーザーID、タイムアウト時間、タイムアウト時刻をセッションに保存する P481<br>
・トークンを使う・・・セッションのExpires属性を使えない場合、クッキーに暗号論的擬似乱数生成系で作った暗号をHttpOnly属性とHTTPSならセキュア属性をつけて保存する<br>
・チケットを使う・・・高度な知識が必要<br>
以上3つで、一番トークンが望ましい。自動ログインを選択しないユーザーに影響がない、複数端末からログインしている場合に一斉にログアウトできる、管理者が特定ユーザーのログイン状態をキャンセルできる、クライアント側に秘密情報が渡らないので解析されない<br>
<br>
エラ〜メッセージは詳しく書かない P489<br>
<br>
ログアウト機能 P492<br>
・副作用があるのでPOSTメソッドでリクエスト<br>
・セッションを破棄する<br>
・CSRF対策する<br>
<br>
【アカウント管理】<br>
・メールアドレスの確認方法<br>
トークン付きURL or 確認番号入力 P497<br>
<br>
自動処理による大量アカウント作成防止　→ Googleのgem CAPTCHA<br>
