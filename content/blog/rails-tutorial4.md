---
title: Railsチュートリアル4章についてアウトプット 前半戦
date: "2022-05-28 15:30:00"
description: "Railsチュートリアルの4章の解説動画を復習をしてRuby、Railsで用意されているメソッドの確認をしていきます.."
---

## はじめに

Railsチュートリアルの4章の解説動画を復習をしてRuby、Railsで用意されているメソッドの確認をしていきます。

※全ては網羅していません

Webテキストの購入者は4章の解説動画だけお試しで見れるらしいので是非確認してみてください。

[https://note.com/yasslab/n/n7a72ec68d797](https://note.com/yasslab/n/n7a72ec68d797)

### 動作確認

Railsチュートリアルに習って`gem pry`を使って動作確認をしていきます。

pryとは簡単にうと、Ruby標準のirbよりも便利にしたものです。

学習する際は、実際に動かしながら挙動を確認すると理解度がダンチなので推奨します。

ここを変えるとどうなるのかな？等も気軽に確認できます。

### 文字列

シングルクォーテーションとダブルクォーテーションの違いは式展開をしないことになります。

```ruby
>> '#{foo} bar'     # シングルクォート内の文字列では式展開ができない
=> "\#{foo} bar"
```

使い所を考えるとシングルを使う場面もでてくるかもしれない。

### 出力

`print`は渡されたものをそのまま表示します。

`puts`はput stringの略。表示した後に改行されます。

```ruby
[5] pry(main)> a = puts "hi"
hi
=> nil
[6] pry(main)> a
=> nil
```

この⇒nilなに？

メソッドの戻値はnilということ。

### nilは他の言語のNULLとは違う

nilもオブジェクトであり、メソッドを呼び出すことが出来る。

```ruby
[7] pry(main)> nil.to_s
=> ""
# nilは文字列に変換すると空文字になる
```

### Rubyとオブジェクト指向について

Rubyの設計思想として、「オブジェクト指向」を高く掲げています。(Ruby製作者のMatsさんはプログラミング言語はそうであってほしいと考えていると思われる。)

Rubyは、ほぼ全てがオブジェクトでできているので、オブジェクト指向を理解するためにRubyを勉強するのはとても有効です。

**「オブジェクトとメッセージの受け渡しでほぼ全てのことを表現できるからオブジェクト指向言語なのである」（安川さん談）**

オブジェクト（数値とか文字列とか論理値とか）に対して「これやってください！」（メソッド）と言ったら何か結果を返してくれます。

例えば下記の例をみると、

```ruby
[10] pry(main)> "foobar".length
=> 6
# 文字数を返してくれる。

[11] pry(main)> "foobar".empty?
=> false
[12] pry(main)> "".empty?
=> true
# 空文字列かどうか論理値を返してくれる。

[9] pry(main)> 1.to_hogehoge
NoMethodError: undefined method `to_hogehoge' for 1:Integer
from (pry):6:in `__pry__'
#　定義されてないものはエラーを返してくれる。
```

このような形でオブジェクトに何かメソッドを投げると、メッセージを返してくれる。

ちなみに、他の言語でオブジェクト指向にしたい場合は、文字列、数値、論理値等にそれぞれクラスを作成していけば、オブジェクト指向にコードを書くことができます。

※余談

Rubyの（ほぼ）**全てはオブジェクトについて**は下記の通り。

```ruby
[20] pry(main)> true.class
=> TrueClass
[21] pry(main)> TrueClass.class
=> Class
[22] pry(main)> TrueClass.class.to_s
=> "Class"
```

- trueはTrueClassクラスのインスタンスである。
- TrueClassはClassクラスのインスタンスである。
- Classクラスもオブジェクトなので、文字列に変換するメソッドなど使える。
- つまり全てはオブジェクトである。

### Rubyは直感的に書ける

Rubyはプログラミングを書く「人間」にとってやさしい言語であるべきと考えられている。

つまり直感的にかけるということです。

**エイリアスがある**

```ruby
[23] pry(main)> "foobar".length
=> 6
[24] pry(main)> "foobar".size
=> 6
```

これによって他の言語を学んで来た人が、こうやったら書けるかな？と予想して書いたらその通り動く、ということがある。

**言語が人間の考え方に合わせてくれる**

コンピュータ的に考えるとif文を書く場合文頭に書くべきだが、Rubyは後置if文が書くことができる。(開発者ががんばってるから後置if文が使える)

```ruby
[26] pry(main)> x = "foo"
=> "foo"
[27] pry(main)> y = ""
=> ""
[28] pry(main)> puts "どっちもから" if x.empty? && y.empty?
=> nil
# なにも表示されない。Falseだから。
# && を　and と書くこともできる。

[29] pry(main)> puts "どっちかがカラ" if x.empty? || y.empty?
どっちかがカラ
=> nil
# trueなので表示される。
# ||は orと書くこともできる。

[30] pry(main)> puts "xは空じゃない" if !x.empty?
xは空じゃない
=> nil
```

※逆に、例えばPythonは誰が書いても同じように書けるという設計思想がある。

### メソッドの定義

回文であるか確かめるメソッドを定義してみます。

```ruby
[36] pry(main)> def palindrome?(s)
[36] pry(main)*   if s == s.reverse
[36] pry(main)*     true
[36] pry(main)*   else
[36] pry(main)*     false
[36] pry(main)*   end
[36] pry(main)* end
=> :palindrome?
# 引数が回文であるならばtrue、違ったらfalseを返すメソッド
```

```ruby
[37] pry(main)> palindrome? "level"
=> true
[38] pry(main)> palindrome? "foobar"
=> false
```

Rubyではif文のreturnは記述する必要がなく、最後に実行されたものを返す。

例えばチームにRubyに慣れていない人がいる場合、returnを書いたほうが理解されやすい。

```ruby
def palindrome?(s="level) #メソッドに引数がない場合はデフォルトの定義をすることもできる。
	if s == s.reverse
		true
	else
		false
	 end
end
```

### おわりに

書き方というよりは言語としての考え方を学べました。
後半戦はもっと実際のコードの挙動等に踏み込んでいくはずなので気を引き締めて頑張ります。

後半戦はまた後日書きます！
