# threastメモ
このリポジトリは、threastアプリケーションの開発において、設計や実装に関するメモを記録するためのものです。
開発途中であるため、内容は変更される可能性があります。開発の実態に基づいて編集されます。

## 要件定義/やりたいこと

### 非ログイン時

できること
 - 投稿一覧を確認(なおいいねを押した人物は表示不可/いいね数は表示/コメントは表示可能)
 - ユーザのプロフィールを確認(制限あり)


とりあえず疑問
 - 投稿のコメントを見る機能として、APIはどのように設計するか
   - 投稿表示APIにコメントを含めるか？ -> 配列で返すことになる？ つけるとしても、ページネーションは手間になるのでつけないことになりそう

### ログイン時

できること
 - 自分のプロフィールを表示
   - 非ログイン時や、自分以外が見たときより表示内容が増える
 - 自分のプロフィールを編集
   - 表示名 -> ユーザネーム
   - スクリーンネーム -> ユニークなID
   - コメント
   - 隠しコメント
   - プロフィール画像？
   - パスワード変更
     - 注意: パスワードは現行パスワードを入力していないと変更できないようにする
 - 自分を削除
   - 削除すると、自分の投稿やコメント、いいねも全て削除されるようにしたい
 - フォロー機能
   - さらにフォローしているユーザの投稿を一覧できるようにしたい
 - 投稿を作成
   - タイトルと本文の指定
   - タグの指定を出来るようにする？
 - 投稿の編集
   - たぶん同じようなAPIのかたちになる
 - 投稿の削除
   - 削除すると、その投稿に対するコメントやいいねも全て削除されるようになる？
 - 投稿に対するいいね機能
 - 投稿に対するコメント機能
   - いいねと同じように、コメントに対するいいね機能もつける・・・？できるの？
   - コメントに対して連続でコメントをつけることをできるようにする？(難しそう)
   - コメント削除機能はつける？
     - 付けるとしたら、コメントに対するいいね機能や、コメントに対するコメント機能と同じように、コメントに対するコメント削除機能もつける？

/*
 - ルーム作成機能
   - ここで追加するユーザも選択できるようにする？
     - FFであるユーザしか追加できないように
 - ルーム削除機能
   - 過去の会話も全て削除されるようにする
 - ルームチャット機能
   - これは一言コメントのようにデータが増えていく感じで
   - しかしコメントのように編集することはできない
   - 
*/

とりあえず疑問と不安点
 - プロフィール画像のアップロード機能含め画像の取得部分までどうすればいいのか？
 - ハンドルは一意でなければいけないことに留意
 - 自分の投稿やコメント、いいねが全て削除されるが、それによって不具合は発生しないか？
 - フォロー機能、セルフリレーションになるけどどう扱えばいいのか？これは…
 - タグの投稿機能をつけるとして、配列でリクエストを送信することになる？
   - また、タグの検索画面とそのAPIも実装することになる？
 - いいね機能のAPIの設計をどうすべき？
 - コメントに対するいいね機能やコメントに対するコメント機能はどうすべき？
   - いいね機能は、投稿に対するいいね機能と同じように実装すればいい？
   - コメントに対するコメント機能は、コメントに対するいいね機能と同じように実装すればいい？
   - そもそも、いいね機能やコメント機能は、投稿に対するものとコメントに対するものとでAPIとデータベースを分けるべき？
   - 削除処理をつけるのだとしたらどうすればいい？
/*
 - ルーム作成時やユーザ管理時に複数ユーザ選択するが、配列で指定するようにするのか
   - そうなるとユーザ検索機能とかも必要になりそうだけども
 - 所属しているルームの表示APIとかも必要？
 - ルーム機能、ユーザに対してN対Nになる気がする
   - APIのレスポンスはどうすればいいんだ？
*/

### 全般の疑問点/不安
 - そもそもidを連番など採用するか、cuidなどのランダムな文字列を採用するか
   - 連番の場合、削除したときにidが空いてしまうので、idを振り直す必要がある
   - cuidの場合、idがランダムな文字列なので、削除したときにidが空いてしまうことはない -> こっちかな
   - cuidなら長さもそれほど気にならないだろうと踏んだ

### アーキテクチャや命名の疑問点/不安
 - 命名をどうすればいい -> ルールを決めておく?
    - 変数名・ディレクトリ名・ファイル名・Prismaスキーマのプロパティ・その他よわい存在 (->ファイル名のスネークに変更 キャメルは威圧感があるので…)
      - 一般的にスネークケース
        - 例: user_name
    - クラス名・関数名その他強い存在
      - 一般的にキャメルケース？
        - 例: UserName
        - 関数名は先頭小文字 クラス名は線という大文字が好ましそう
  - フロントエンドは別にパスベースでもいいと思う
    - バックエンドはモジュールでまとめる形なのでコントローラにパスを記述する方式になりそう？
  - dtoの名称をどの順番にすればいいんだろうか…？
    - 現在パスと逆順序になっていて、あんまよくない気がする

### データベース設計の疑問点/不安
 - どのようなプロパティを用意したらいい？
   - ユーザ
     - cuid
     - ユーザネーム
     - スクリーンネーム
     - ビオ
     - 隠しコメント
     - プロフィール画像
     - パスワード
     - フォローしているユーザのidの配列
     - フォローされているユーザのidの配列
       - セルフリレーションどうする
   - 投稿
     - cuid
     - タイトル
     - ボディ
     - プライベートフラグ


### API設計の疑問点/不安 (nest.js)
 - nest generatorは使わないでいく方針を考える
 - レスポンスのバリデーションはやっぱりどうすればいいのか
 - コントローラとサービスの使い分け、どちらをどこまで踏み込ませればいいのか
   - どこまでがコントローラのエリア？そしてどこからがサービス？
     - 多分セッションとかの内容は引き継がなくてもいいとは思う
     - サービスをDIできると嬉しい？と考えると、サービスはユーザ定義関数のように使えればいいのかもしれない
       - つまり、必要な値だけを渡してロジックとして処理をしてもらう
 - ディレクトリ構成をどうするか
   - 似たようなモジュールを一つのディレクトリとしてまとめるか？ -> ディレクトリがみにくくなる・・・？
     - ので、特に考えずにモジュールはフラットにディレクトリを並べることにする
   - ディレクトリの命名をどうするか
     - 同じエンドポイントでもログインしているかどうかでモジュールを分けるとする
     - このときにサブディレクトリにネストしてしまっていい？->禁止
     - dtoを持たせるときはcommonsで共通化？
 - dtoはどのように設計すればいいのか？？
   - ~~ネストすることもあるかもしれない~~ないかも
     - ネストするとき、オブジェクトの他のdtoから持ってくるのか？
     - dtoの定義やスキーマもあらかじめしっかり設計しておいた方がいいのかも
     - というか、ページネーション入れるんだったらメタデータ構造と分けてネストせざるをえないかも
     - メタデータ用のdtoも設計？
       - 例: 
        {
            "metadata": {
                "total": 100,
                "page": 1,
                "per_page": 10
            },
            "items": [
                {
                    "id": 1,
                    "name": "hoge"
                },...
            ]

        }
      - こうしたときのサービスとコントローラはどうすればいいんだ
    - やっぱり重要なdtoをcommonsディレクトリにおいてエイリアス配置しちゃえばええか・・？
    - ->どうやらopenapiはスキーマとなるdtoの組み合わせを表現することが可能みたい

## テーブル定義
 - user -> (cuid, handleでユニーク)
   - cuid (private)
   - created_at (private)
   - updated_at (private)
   - handle (unique)
   - screen_name
   - bio
   - hidden_comment
   - profile_image(?)
   - password(argon2)
   - role union((admin, subadmin, manager, user))
   - following[] (user:following = 1:N) (onDelete: cascade, ユーザが削除されたらフォローも削除)
     - ユーザは複数のフォローベクトルを生やすことがあるため
   - followed[] (user:followed = 1:N) (onDelete: cascade, ユーザが削除されたらフォローも削除)
     - ユーザは複数のフォローベクトルを受けることがあるため
   - posts[] (user:posts = 1:N) (onDelete: cascade, ユーザが削除されたら投稿も削除)
   - post_likes[] (user:post_likes = 1:N, post:post_likes = 1:1) (onDelete: cascade, ユーザが削除されたらいいねも削除)
     - いいねが複数のいいね主を持つことはなく、しかし1人のユーザが1つの投稿に対して1回しかいいねできないので
   - comment[] (user:comment = 1:N, post:comment = 1:N) (onDelete: cascade, ユーザが削除されたらコメントも削除)
     - コメントが複数のコメント主を持つことはなく、コメント主が複数のコメントを持つこともないので
/*   - rooms[] (rooms:users = N:N)
     - ユーザが複数のルームに所属することがあり、ルームには複数のユーザが所属することがあるので
   - messages[] (room:messages = 1:N, user:messages = 1:N)
     - メッセージが複数のメッセージ主を持つことはなく、メッセージが複数のルームを持つこともないので
*/

 - follow -> (cuidでユニーク, src_user_cuid&dst_user_cuidでユニーク) (これは中間テーブル)
   - cuid (private)
   - created_at (private)
   - updated_at (private)
   - src_user_cuid <- user (攻め) (user:following = 1:N)
   - dst_user_cuid <- user (受け) (user:followed = 1:N)

 - post -> (cuidでユニーク) 
   - cuid (private)
   - created_at (private)
   - updated_at (private)
   - title
   - body
   - private
   - hashtags[] (post:hashtag = N:N) (onDelete: cascade, ポストが削除されたらハッシュタグの中間テーブル削除)
     - ハッシュタグが複数のポストに付与されることがあり、またポストが複数のハッシュタグを付けることもあるため
   - src_user_cuid <- user (user:posts = 1:N)
   - 

 - hashtag -> (cuid, nameでユニーク)
   - cuid (private)
   - created_at (private)
   - updated_at (private)
   - name ((unique))
   - description
   - posts[] (hashtag:posts = N:N) (onDelete: cascade, ハッシュタグが削除されたらポストの中間テーブル削除)

 - like -> (cuid, src_user_cuid&dst_post_cuidでユニーク)
   - cuid (private)
   - created_at (private)
   - updated_at (private)
   - src_user_cuid <- user (user:like = 1:N)
   - dst_post_cuid <- post (post:like = 1:1)

 - comment -> (cuidでユニーク)
   - cuid (private)
   - created_at (private)
   - updated_at (private)
   - content
   - src_user_cuid　<- user (user:comment = 1:N)
   - dst_post_cuid　<- post (post:comment = 1:N)

/*
 - room -> (cuid, )
   - cuid (private)
   - created_at (private)
   - updated_at (private)
   - name
   - description 
   - users[] (rooms:users = N:N) 
      - ユーザが複数のルームに所属することがあり、ルームには複数のユーザが所属することがあるので

 - message
   - cuid (private)
   - created_at (private)
   - updated_at (private)
   - content
   - src_user_cuid <- user (user:message = 1:N)
   - src_room_cuid <- room (room:message = 1:N)
*/

N:NリレーションのときはPrismaが中間テーブルを作成するためどちらもリストとしたプロパティでいいはず
プロパティに任意のときは(?)マークで任意であることを明記
onDeleteがCascadeであるなら明記

## dto定義
 - req
   - login_form (これは例外)
     - login.ts -> LoginFormReqDto
       - handle
       - password
   - user
     - create.ts -> CreateUserReqDto
       - handle 3~20 string
       - screen_name 1~40 string
       - password StrongPassword
     - update.ts -> UpdateUserReqDto (idはログイン済id or パスクエリよりcuid指定)
       - screen_name? 1~40
       - handle? 3~20
       - bio? 1~100
       - hidden_comment? 1~100
   - user_password 
     - update.ts -> UpdateUserPasswordReqDto (idはログイン済id or パスクエリよりcuid指定)
       - old_password
       - new_password
   - user_image 
     - update.ts -> UpdateUserImageReqDto (idはログイン済id or パスクエリよりcuid指定)
       - ???
   - post
     - create.ts -> CreatePostReqDto
       - title 1~100
       - body 1~1000
       - private boolean
       - hashtags[] -> String(cuidを導入)存在しなければエラーにする
     - update.ts -> UpdatePostReqDto (idはログイン済id or パスクエリよりcuid指定)
       - title?
       - body?
       - private?
       - hashtags[] -> String(cuidを導入)存在しなければエラーにする
   - hashtag
     - create.ts -> CreateHashTagReqDto
       - name 1~20
       - description 1~100
     - update.ts -> UpdateHashTagReqDto (idはログイン済id or パスクエリよりcuid指定)
       - name?
       - description?
   - post_comment
     - create.ts -> CreatePostCommentReqDto
       - content 1~300
       - (dst_post_cuidはパスクエリで指定)
         (ユーザはログインしているのでsrc_user_cuidはロジックにより挿入)
     - update.ts -> UpdatePostCommentReqDto (idはログイン済id or パスクエリよりcuid指定)
       - content 1~300 (これってcuidをパスクエリで指定するのか)
     
 - res
   - user
     - tiny.ts -> TinyUserResDto
       - cuid
       - screen_name
       - handle
     - limited.ts -> LimitedUserResDto
       - cuid
       - screen_name
       - handle
       - bio
       - created_at
       - updated_at
     - standard.ts -> StandardUserResDto
       - cuid
       - screen_name
       - handle
       - bio
       - hidden_comment
       - role (admin, subadmin, manager, user) -> stringのunionになりそう
       - created_at
       - updated_at
     - full.ts -> StandardUserResDto
       - cuid
       - screen_name
       - handle
       - bio
       - hidden_comment
       - role (admin, subadmin, manager, user) -> stringのunionになりそう
       - created_at
       - updated_at
   - post
     - tiny.ts -> TinyPostResDto
       - cuid
       - title
     - limited.ts -> LimitedPostResDto
       - cuid
       - title
       - body
       - created_at
       - updated_at
       - user -> TinyUserResDto
       - hashtags[] -> TinyHashTagResDto[]
     - standard.ts -> StandardPostResDto
       - cuid
       - title
       - body
       - created_at
       - updated_at
       - user -> TinyUserResDto
       - hashtags[] -> TinyHashTagResDto
       - likes -> (いいねされた数 うーんって感じかもしれんがここはロジックで頑張るしか)
       - comments -> (コメントされた数 ここも同じ)
   - hashtag
     - tiny.ts -> TinyHashTagResDto
       - cuid
       - name
     - standard.ts -> StandardHashTagResDto
       - cuid
       - name
       - description
       - created_at
       - updated_at
   - post_like
     - standard.ts -> StandardPostLikeResDto
       - cuid
       - created_at
       - updated_at
       - user -> TinyUserResDto
       - post -> TinyPostResDto
   - post_comment
     - standard.ts -> StandardPostCommentResDto
       - cuid
       - content
       - created_at
       - updated_at
       - user -> TinyUserResDto
       - posts -> TinyPostResDto
   - wrapper
     - paginated.ts -> PaginatedResDto
       - per_page
       - current_page
       - data(unknown)
     - PaginatedResDtoTinyUser
     - PaginatedresDtoTinyPost


これ、対象をディレクトリとして分けるべきでは？？RESTの思想的に
そしてUserのPasswordというようにリソースが所属する形で分けている(reqに限るかも)
データベースには存在していない要素であってもリソースには入れている。必ずしも一致するわけではないので…
アカウント作成時には、ユーザー名とパスワードのみを受付でええよな
~~ログインしている状態でのdtoについては、接頭語としてAuthedを用いることにする~~
こうではなく、limitedとstandardでわけることにする -> fullがいつか作られるときのために考える
  -> fullは形だけでも作っておくことにする adminの閲覧のため
~~これ、Authedだと認識の違いが発生しやすいのでは？~~
ファイルに落とし込むときって命令どうすればみやすくなるのか…？
~~これ、基本となるDTOを用意して、それを継承しつつpartialだのrequiredだのを指定し、デコレータも付与するようにしていくと良さそう？~~過激なDRY原則の適用はしんどさを生む
ハッシュタグの内容を表示するAPIで、ハッシュタグに紐づいた投稿一覧を返すか？->APIを分けたほうが良さそう *解決済み
というか、ハッシュタグで絞り込みをして投稿一覧表示するのはpostのgetに任せることにすればいい説
postの執筆ユーザについては、展開して返すかuserのdtoを含めるかどうする？
他のdtoにネストされる前提のものはtinyという接頭語を用いてみる
リクエストの方はできるだけネストしたくないよなぁ
いいねしたユーザのリストを取得するAPIは別に必要だよなぁ
likesが中間テーブルみたいな感じになるんかこれ まあそうか…そのためlikeにはtinyは存在せず？(likeテーブル自体は返す必要がない中間テーブルなので)
createとupdateで、パスのクエリからcuidを指定可能だから差異が発生するのでは？
followのdtoは要らないだろうので削除した クエリで十分だろうと判断(この類の判断は意外と多い というかdeleteリクエストとか全部そうかも)
配列からtypeofでenumを生成->class-validatorのISEnumに配列を指定してバリデーション？
Prismaについてはstringのunionなので心配ご無用
本当にページネーションってどうするんだ、Prismaと連携？
ページネーション、やっぱりupdate_atプロパティの降順で並べていくことになる・・・？
まあ普通にオフセットパターンになりそうだなぁ…な気がする。それならPrismaの機能がそのまま使えそうな気がしている
命名の順番を本当にどうするべき？？すべて逆順にしてディレクトリ階層をそのまま適用するか？？うーん…でも・・・
ちょっと待った、ファイル名とクラス名は一致させるべきでは？
wrapperをresディレクトリ以下に移動

## APIを定義
 - auth
   - login
     - POST
       - req
         - LoginFormReqDto
       - res
         - TinyUserResDto
   - logout
     - POST
       - req
         - none
       - res
         - TinyUserResDto
   - signup
     - POST
       - req
         - CreateUserReqDto
       - res
         - LimitedUserResDto
 - users
   - me
     - GET
       - res
         - StandardUserResDto
     - PUT
       - req
         - UpdateUserReqDto
       - res
         - StandardUserResDto
     - DELETE
       - res
         - StandardUserResDto
   - me/password
     - PUT
       - req
         - UpdatePasswordReqDto
       - res
         - StandardUserResDto
   - me/image
     - PUT
       - req
         - UpdateImageReqDto
       - res
         - StandardUserResDto
   - me/followings
     - GET
       - res
         - TynyUserResDto[]
   - me/followings/@{handle}
     - POST
       - res
         - TinyUserResDto
   - me/followers
     - GET
       - res
         - TinyUserResDto[]
   - me/posts
     - GET
       - res
         - TinyPostResDto[]
   - /
     - GET
       - res
         - TinyUserResDto[] (Limitedにする？？どうする？)
   - @{handle}
     - GET
       - res
         - LimitedUserResDto
   - @{handle}/posts
     - GET
       - res
         - TinyPostReqDto[]
   - @{handle}/followings
     - GET
       - res
         - TinyUserResDto[]
   - @{handle}/followers
     - GET
       - res
         - TinyUserResDto[]
 - posts
   - /
     - GET
       - res
         - TinyPostResDto[]
   - {post_cuid}
     - GET
       - res
         - StandardPostResDto
     - PUT
       - req
         - UpdatePostReqDto
       - res
         - StandardPostResDto (ただし他人の投稿であれば403)
     - DELETE
       - res
         - StandardPostResDto (ただし他人の投稿であれば403)
   - {post_cuid}/likes
     - GET
       - res
         - TinyUserResDto[]
     - POST
       - req
         - none
       - res
         - TinyPostResDto
     - DELETE
       - res
         - TinyPostResDto (ただしいいねしていなければ400)
   - {post_cuid}/comments
     - GET
       - res
         - TinyCommentResDto[]
     - POST
       - req
         - CreateCommentReqDto
       - res
         - TinyCommentResDto
 - comments 
   - {comment_cuid}
     - PUT
       - req
         - UpdateCommentReqDto
       - res
         - TinyCommentResDto (ただし他人のコメントであれば403)
     - DELETE
       - res
         - TinyCommentResDto (ただし他人のコメントであれば403)


ページネーションするとき、wrapperに包む？現時点では配列だが
他人の投稿であれば403にするという部分はしっかりロジックで制御
コメントを一つだけ参照する機会とかなさそうだとおもうんやけども
1:1で紐づくもの(ex.いいね)はposts以下にネストしてしまうことができるが
1:Nで紐づくもの(ex.コメント)はネストできないため、updateとdeleteについては別のapiに切り出す必要がありそう
(すべてのコメントをpostにこだわらず取得する需要はないと判断)
deleteした後は空オブジェクトを返す？

## ロジック注意事項
↑充実させる
セッションの構造体の設計を考える
{
  cuid
  handle
  screen_name
}
とりあえずこんなもんかも

## nestjsファイル&ディレクトリ構成
 - src
   - modules
     - Auth (わける必要あるかこれ)
       - nonauth (ここのクラス名/ファイルの命名どないすねん)
         - login
         - signup (signupはここに入れるべきなのか？)
       - auth
         - logout
     - MyProfile
       - auth
         - AuthMyProfile
           - getMe
           - updateMe
           - deleteMe
           - updateMyPassword
           - updateMyImage // これはどうするべき？？？
           - getMyFollowings
           - getMyFollowers
           - createMyFollowing
           - getMyPosts // privateも表示する
     - ProfileByHandle
       - nonauth
         - NonAuthProfileByHandle
           - getAllUsers
           - getUserByHandle
       - auth
         - AuthProfileByHandle
           - getUserFollowingsByHandle
           - getUserFollowersByHandle
           - getUserPostsByHandle
     - Posts
       - nonauth
         - NonAuthPosts
           - getAllPosts
           - getPostByCuid
       - auth
         - AuthPosts
           - createPost
           - updatePostByCuid
           - deletePostByCuid
     - PostLikes
       - auth
         - AuthPostLikes
           - getPostLikesByPostCuid
           - createPostLikesByPostCuid
           - deletePostLikesByPostCuid
     - PostComments
       - auth
         - AuthPostComments
           - getPostCommentsByPostCuid
           - createPostCommentsByPostCuid
           - updatePostCommentsByPostCommentCuid
           - deletePostCommentsByPostCommentCuid
   - submodule
     - Prisma
       - PrismaModule
       - PrismaService
   - guards
     - session
       - SessionGuard
       - SessionGuardModule
       - SessionGuardService
       - setSessionGuard
     - role
       - RoleGuard
       - RoleGuardModule
       - RoleGuardService
       - SetRoleGuard
       - SetRole
   - dto
     - req
       - ...
     - res
       - ...
     - wrapper
     - types
       - exception
   - exceptions
     - SessionNotFoundException.ts
     - RolePermissionException.ts
     - ItemNotFoundException.ts
     - ItemAlreadyExistsException.ts
     - PasswordMismatchException.ts
   - filters
     - prisma
       - PrismaExceptionFilter.ts

こうまとめるのがいいかも？
モジュールのディレクトリ構造を考える
 - どのような構造が一番脳に負担がかからないようにできるか？
 - →一応モジュールを分割してみた
   - authedとunauthedで分ける？どちらを先にディレクトリとして配置？？
バックエンドはfastifyにしてみる？？(ややこしくなるとつらいが…)
  ->　そうなったときの課題
  - セッションを用いるguardの実装
    - `context.switchToHttp().getRequest();`を使ってrequestオブジェクトを取得しているがこれをfastifyでやるには
サービス内部での命名規則はどうすればいいのか
 - クラス名は先頭大文字/メソッドは先頭小文字
 - モジュールの分割単位はどうする？？
 - ~~どのリソースを返却するかに応じてモジュールを分割する方針で行く~~
 - しかし、これある程度はapiに縛られるはず(コントローラでルーティングパスを指定するので)
   - うーむどうする
   - やはり恩恵を感じないため、apiに沿ってモジュールを分割することにする
   - dtoの形を変えたければdtoを変えればいいだけ
 - コメント等は複数apiが存在するが、これをもっと別のapiであることを明確に示すべき・・・？
自然数というバリデーションを忘れずに
モジュールにおいてのファイル名は{クラス名スネークケース}_mo で統一するのがいいかもしれない
変数・引数の命名
 - ユーザのセッションのcuid
 - リクエストとして与えたパラメータ類
ページネーションの変数名はwrapperと揃える
 - current_page
 - per_page

なんか権限管理が死ぬほど怠くなってきている
後でTODOとして…caslで何とか出来るところはする


## ページ推移構成

 - /login
 - /signup
 - /mypage
   - /mypage/profile
   - /mypage/password
   - /mypage/image
   - /mypage/followings
     - ?page=1
   - /mypage/followers
     - ?page=1
   - /mypage/posts
     - ?page=1
 - /users/{handle}
   - /users/{handle}/profile
   - /users/{handle}/followings
   - /users/{handle}/followers
   - /users/{handle}/posts
     - ?page=1
 - /posts
   - /posts/{post_cuid}
     - /posts/{post_cuid}/likes
       - ?page=1
     - /posts/{post_cuid}/comments
       - ?page=1
 - /comments/{comment_cuid}
   - /comments/{comment_cuid}/likes
     - ?page=1

## react設計メモ

 - ログインしているアカウント情報を保持する
   - ログインしているアカウント情報を保持するためのatomを作成する
   - 利用するライブラリはjotai
   - 保持する内容はTinyUserDtoの内容と同じ
     - cuid
     - handle
     - user_name
   - 
 - orvalで@tanstack/react-queryの型を生成する

ヘッダとフッダを用意してNested Routingということになるのかな？
 - ヘッダとフッダはどのページでも表示されるので、それらを囲むようにページを作成する
利用するUIライブラリはradix UIがいい？radix-ui/react-*シリーズでなんとかしたい

## reactカスタムフック案

## reactコンポーネント案

## reactページ案