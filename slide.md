### 表参道.rb #43
- - -

## Ruby にコミットしよう

---

### 自己紹介
- - -

* なまえ  : おしょー
* Twitter : [@pink_bangbi](https://twitter.com/pink_bangbi)
* github  : [osyo-manga](https://github.com/osyo-manga)
* ブログ  : [Secret Garden(Instrumental)](http://secret-garden.hatenablog.com)
* Rails 歴 1年の初心者
* 2ヶ月ぐらいずーっと ActiveRecord の実装を読んでる

---

#### 進捗
- - -

* Ruby 2.6.0
  * 取り込まれたパッチ : 3
  * 取り込まれなかったパッチ : 5
  * 報告・修正したバグ : 2
  * 埋め込んだバグ : 1
* Ruby 2.6.1
  * 報告・修正したバグ : 1
* Ruby 2.7.0
  * 取り込まれたパッチ : 1

---

#### 進捗内容
- - -

* refinements の緩和案
  * [`#public_send`](https://bugs.ruby-lang.org/issues/15326)
  * [`#respond_to?`](https://bugs.ruby-lang.org/issues/15327)
  * [`#method` `#instance_method`](https://bugs.ruby-lang.org/issues/15373)
  * [ブロック引数で `&:hoge` 渡した場合](https://bugs.ruby-lang.org/issues/15114)
* `#===` の追加
  * [`Array#===`](https://bugs.ruby-lang.org/issues/14916)
  * [`Hash#===`](https://bugs.ruby-lang.org/issues/14869)
* `{ id, name, age }` 記法の提案
  * [`#expand(:id, :name, age)`](https://bugs.ruby-lang.org/issues/15286)
  * [`%h(id name age)`](https://bugs.ruby-lang.org/issues/14973)

---

### 今日話すこと
- - -

## Ruby にコミットしよう

---

#### Ruby にコミットされるまでの流れ
#### 〜 新しい機能を提案する場合 〜
- - -


1. Ruby に必要か考える
1. パッチを書く
1. bugs.ruby で提案する
1. 開発者会議の議題で取り上げてもらう
1. 問題があれば修正する
1. matz がいいよって言えばコミットされる


---

### 実例（Ruby 2.5 時点での挙動）
- - -

```ruby
module Ex
  refine String do
    def twice
      self + self
    end
  end
end

using Ex

p "homu".send(:twice) # OK
p "homu".public_send(:twice) # NG
```

* `#send` では refinements が有効になるが `#public_send` では有効ならない
* `#public_send` でも有効にしたい

---

### Ruby に必要か考える
- - -

* ユースケースを考える
  * より具体的なケースを提示する
  * → `send` よりも `public_send` が安全
* 一貫性があるか
  * 他の機能と比べて整合性が取れているか
  * → `#send` で動作して `#public_send` で動作しないのは一貫性がない
* その機能、本当に Ruby に必要？
  * gem ではダメ?
  * → 今回は本体に手を入れないと難しい…

---

### パッチを書く
- - -

* Ruby 本体に対して実装する
* Ruby は C言語で書かれているので C言語の知識が必要
* Ruby をハックする上で [Cookpad Ruby Hack Challenge](https://github.com/ko1/rubyhackchallenge) が参考になる
* パッチを書くのが難しければまずは提案からでも大丈夫
  * 提案によっては先に議論から入ったほうがいいケースもある
* [今回書いたパッチ](https://github.com/ruby/ruby/pull/2019/files)

---

### bugs.ruby で提案する
- - -

* [bugs.ruby](https://bugs.ruby-lang.org/projects/ruby-trunk/issues)で新しい issues を建てる
* 英語でも日本語でも OK
  * [報告先] を選ぶ必要があるので注意
* パッチファイルを添付するか github に pull request を投げて、その URL を貼る
  * github に pull request を投げると CI が走るので便利
* [今回建てた issues](https://bugs.ruby-lang.org/issues/15326)

---

### 開発者会議の議題で取り上げてもらう
- - -

* 毎月コミッタの人たちが集まって会議している
  * 提案された内容などについての議論を行っている
* この会議で議論してもらえるように [issues](https://bugs.ruby-lang.org/projects/ruby-trunk/issues?query_id=156) に追加する必要がある
  * [今月の開発者会議](https://bugs.ruby-lang.org/issues/15546)
* この会議で問題ないと判断されればコミットされる


---

### おまけ1 : Ruby をハックしたいなら
- - -

* メーリングリストに登録する
  * bugs.ruby の情報が流れてくる
* [Ruby Hack Challenge](https://connpass.com/event/119128/) に参加する
  * もくもく会

---


### おまけ2：最近あった面白い議論
- - -

* [`#method` のシンタックスシュガー](https://bugs.ruby-lang.org/issues/13581)
  * `obj.method(:hoge)` が `obj.:hoge`
* [`{ name, age }` みたいな省略記法](https://bugs.ruby-lang.org/issues/15236)
  * [`{ name: name, age: age }` を `{ name, age }`]
* [`Symbol` で関数合成](https://bugs.ruby-lang.org/issues/15483)
  * `map { |it| it.to_t.chr }` を `map(&:to_i >> :chr)`

---

## まとめ

* Ruby の開発プロセスについて説明した
* メーリングリストを見てると面白い議論が流れてくる
* Ruby を Hack したいなら Ruby Hack Challenge に参加してみよう

---

## ご清聴
## ありがとうございました
