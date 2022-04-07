---
title: Railsチュートリアル基礎力向上テスト〜六〜
date: "2022-04-07 22:30:00"
description: "就活同期との面接練習のおかげで面接恐怖症が減ったおおのです…"
---
今日もRUNTEQ同期と面接練習をして、明らかに相手の方が面接上手なので、まるでキャリアトレーナーかのようにフィードバックをもらっています。僕は大したフィードバック言えてない気がします。申し訳ない気持ちで一杯です。他の分野でお返ししたいと思っているおおのです。<br>

### QiitaNight【2022年のRailsの開発現場事情について語ろう！】
参加しました。<br>
https://increments.connpass.com/event/241385/
<br>

印象に残った言葉は、「Railsは毎年死んでいる(オワコンと言われている)。それでも着実に進化は遂げている」という言葉。なんか笑ってしまいました。
当たり前のことかもしれませんが、Railsが得意としていることはRailsで、不得意な分野は他の技術を選択していくのが良い。とのことです。
色々な技術に触れてサービスによってしっかりと技術選定ができるニンゲンになりたいですね。がんばります。

##  Railsチュートリアル完走者向け基礎力向上テスト〜六〜
それではRailsチュートリアルの『基礎力向上テスト』を深堀りしていきます。

最初は正直に自分なりの考えを簡単に書いて、その後に調べたことを(主に安川さんの解説を基に)書いていきます。
 宣伝を兼ねて参考にしているリンクを貼っておきます。 https://railstutorial.jp/screencast

### Q11.第11章「send メソッドを使う理由」
**Q. 本章では、Ruby が提供する強力なメソッド『send』を使って、remember_digest にも activation_digest にも使える改良版メソッド『authenticated?』を実装しました。この authenticated? メソッドを改良するために、なぜ send メソッドを使ったのでしょうか？その理由を説明してください。**
> Railsチュートリアル11章より引用
```ruby
# app/models/user.rb
  # トークンがダイジェストと一致したらtrueを返す
  def authenticated?(attribute, token)
    digest = send("#{attribute}_digest")
    return false if digest.nil?
    BCrypt::Password.new(digest).is_password?(token)
  end
```

(おおの)A.11章のタイトルはアカウントの有効化。メールを使ってアカウントを有効化させるやつですね。この章も難しかった記憶があるし、一周しかしていないので全然覚えていません。(Be Openの精神)
内容確認してみます。


### A. send メソッドを使用して抽象化することで呼び出すメソッドを動的に決めることができる 。それを利用してauthenticated? メソッドを他の認証でも使えるようにするため。
**authenticated?メソッドを抽象化している。例えば下記のように**
- ログイン認証には```remember_token, remember_digest```を使う
- 有効化認証には```activation_token, activation_digest```を使う
- 再設定認証には```reset_token, reset_digest```を使う
この様に使い分けたい時に上記のようなauthenticated?メソッドを実装する。sendメソッドを使って文字列を式展開する。if文を使わずにシンプルな実装が可能となる上、増やしたい場合も容易である。
こういった実装をメタプログラミングの一種、動的ディスパッチなどと言うらしい。メソッド名を予め予想しておかないとこういった実装はできないため、難易度が高い。

動的ディスパッチについて参考<br>
https://qiita.com/y4u0t2a1r0/items/8e19ca364ecba5c67ee5

面白いけど難しい。Railsチュートリアルってこんなに難しかったんですね。笑えます。
安川さんはこのauthenticated?メソッドの説明好きらしい。やはり内容的に難しいとのこと。現場でこういう実装ができたらかなりレベル高いですとのこと。メタプログラミングとしては入門レベルらしい。。

### Q12 第12章「hidden_field を使う理由」
**Q.パスワード再設定のフォームでは『hidden_field_tag』を使ってメールアドレス情報を渡しています。なぜ『hidden_field_tag』を使っているのでしょうか? その理由を説明してください。**
> Railsチュートリアル12章より引用
```ruby
# app/views/password_resets/edit.html.erb
<% provide(:title, 'Reset password') %>
<h1>Reset password</h1>

<div class="row">
  <div class="col-md-6 col-md-offset-3">
    <%= form_with(model: @user, url: password_reset_path(params[:id]),
                  local: true) do |f| %>
      <%= render 'shared/error_messages' %>

      <%= hidden_field_tag :email, @user.email %>

      <%= f.label :password %>
      <%= f.password_field :password, class: 'form-control' %>

      <%= f.label :password_confirmation, "Confirmation" %>
      <%= f.password_field :password_confirmation, class: 'form-control' %>

      <%= f.submit "Update password", class: "btn btn-primary" %>
    <% end %>
  </div>
</div>
```

(おおの)A.既にログインしておりメールアドレスは取得済みのため、わざわざユーザーに入力させる必要が無い。ユーザーフレンドリーにするためにそうしている。

### A. パスワード再設定のフォームが表示される際に email 情報を取得しているため、その情報を利用したいから。すでに取得している情報を利用するので、ユーザーに入力してもらう手間をかけずに済む。
**paramsの中にメールアドレスが入っているので、入力させる必要がない。```<%= hidden_field_tag :email, @user.email %>```のように記述し、@userに入っている情報を再利用する。そしてhidden_fieldを使うことで見せなくしている。そうすることでユーザー体験がより良くなる。**

11問目が難しかったので今回は易しめでよかった。。！

### おわりに
そういえばテスト(RSpec)書かなきゃ..!と急に強く思ったので明日から早速テスト書きます。<br>
個人開発したアプリに書いていこうと思います。多分それが一番勉強になる。
正直苦手だったので後回しにしていたけど、Railsチュートリアルでテストの大事さをとても語っていて思い出したのでそうも言ってられませんね。逆に楽しみになってきた。<br>
RSpecはいいぞおじさんになれるように頑張ります。
