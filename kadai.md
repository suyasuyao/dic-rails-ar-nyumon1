# 5.小課題1
最後のレコードだけを取り出すコードを記述してください。

```
irb(main):003:0> Score.all.last
  Score Load (1.7ms)  SELECT  "scores".* FROM "scores" ORDER BY "scores"."id" DESC LIMIT $1  [["LIMIT", 1]]
=> #<Score id: 40, name: "katori", subject: "Music", score: 75, created_at: "2020-01-11 10:20:19", updated_at: "2020-01-11 10:20:19">
```

# 6.小課題2
最後のレコードのsubjectのみを取得するコードを記述してください。
```
irb(main):009:0> Score.all.last.subject
  Score Load (2.6ms)  SELECT  "scores".* FROM "scores" ORDER BY "scores"."id" DESC LIMIT $1  [["LIMIT", 1]]
=> "Music"
irb(main):010:0> Score.select("subject").last
  Score Load (1.8ms)  SELECT  "scores"."subject" FROM "scores" ORDER BY "scores"."id" DESC LIMIT $1  [["LIMIT", 1]]
=> #<Score id: nil, subject: "Music">
```


# 7.小課題3
最後のレコードのscoreの値を70に更新するコードを記述してください。
```
Score.last.update(score:70)
  Score Load (1.9ms)  SELECT  "scores".* FROM "scores" ORDER BY "scores"."id" DESC LIMIT $1  [["LIMIT", 1]]
   (1.1ms)  BEGIN
  Score Update (1.1ms)  UPDATE "scores" SET "score" = $1, "updated_at" = $2 WHERE "scores"."id" = $3  [["score", 70], ["updated_at", "2020-01-11 10:37:38.463954"], ["id", 40]]
   (1.5ms)  COMMIT
=> true
```

# 8.小課題4
scoresテーブルに新しく以下の値をもったレコードを作成するコードを作成してください。

name DIVE
subject English
score 90

```
irb(main):013:0> @score = Score.new(name:"DIVE", subject:"English", score:90)
=> #<Score id: nil, name: "DIVE", subject: "English", score: 90, created_at: nil, updated_at: nil>
irb(main):014:0> @score.save
   (1.5ms)  BEGIN
  Score Create (1.6ms)  INSERT INTO "scores" ("name", "subject", "score", "created_at", "updated_at") VALUES ($1, $2, $3, $4, $5) RETURNING "id"  [["name", "DIVE"], ["subject", "English"], ["score", 90], ["created_at", "2020-01-11 10:40:00.419171"], ["updated_at", "2020-01-11 10:40:00.419171"]]
   (2.0ms)  COMMIT
=> true
```

# 9.小課題5
kimuraさんのEnglishのレコードのみを取得するコードを書いてください。

(ヒント)
whereメソッドを使用してみましょう。

```
irb(main):019:0> Score.where(name:"kimura", subject:"English")
  Score Load (1.5ms)  SELECT  "scores".* FROM "scores" WHERE "scores"."name" = $1 AND "scores"."subject" = $2 LIMIT $3  [["name", "kimura"], ["subject", "English"], ["LIMIT", 11]]
=> #<ActiveRecord::Relation [#<Score id: 5, name: "kimura", subject: "English", score: 55, created_at: "2020-01-11 10:20:11", updated_at: "2020-01-11 10:20:11">, #<Score id: 25, name: "kimura", subject: "English", score: 55, created_at: "2020-01-11 10:20:19", updated_at: "2020-01-11 10:20:19">]>
```


# 10.小課題6
nakaiさん以外で点数の低い順に10件レコードを取得するコードを書いてください。

(ヒント)
where.notメソッドとorderメソッドとlimitメソッドを使用してみましょう。

```
irb(main):035:0> Score.where.not(name:"nakai").order(:score).limit(10)
  Score Load (1.8ms)  SELECT  "scores".* FROM "scores" WHERE "scores"."name" != $1 ORDER BY "scores"."score" ASC LIMIT $2  [["name", "nakai"], ["LIMIT", 10]]
=> #<ActiveRecord::Relation [#<Score id: 13, name: "inagaki", subject: "English", score: 30, created_at: "2020-01-11 10:20:11", updated_at: "2020-01-11 10:20:11">, #<Score id: 33, name: "inagaki", subject: "English", score: 30, created_at: "2020-01-11 10:20:19", updated_at: "2020-01-11 10:20:19">, #<Score id: 37, name: "katori", subject: "English", score: 40, created_at: "2020-01-11 10:20:19", updated_at: "2020-01-11 10:20:19">, #<Score id: 17, name: "katori", subject: "English", score: 40, created_at: "2020-01-11 10:20:11", updated_at: "2020-01-11 10:20:11">, #<Score id: 9, name: "kusanagi", subject: "English", score: 50, created_at: "2020-01-11 10:20:11", updated_at: "2020-01-11 10:20:11">, #<Score id: 29, name: "kusanagi", subject: "English", score: 50, created_at: "2020-01-11 10:20:19", updated_at: "2020-01-11 10:20:19">, #<Score id: 25, name: "kimura", subject: "English", score: 55, created_at: "2020-01-11 10:20:19", updated_at: "2020-01-11 10:20:19">, #<Score id: 5, name: "kimura", subject: "English", score: 55, created_at: "2020-01-11 10:20:11", updated_at: "2020-01-11 10:20:11">, #<Score id: 28, name: "kimura", subject: "Music", score: 60, created_at: "2020-01-11 10:20:19", updated_at: "2020-01-11 10:20:19">, #<Score id: 8, name: "kimura", subject: "Music", score: 60, created_at: "2020-01-11 10:20:11", updated_at: "2020-01-11 10:20:11">]>
irb(main):036:0> Score.where.not(name:"nakai").order(score:"ASC").limit(10)
  Score Load (2.1ms)  SELECT  "scores".* FROM "scores" WHERE "scores"."name" != $1 ORDER BY "scores"."score" ASC LIMIT $2  [["name", "nakai"], ["LIMIT", 10]]
=> #<ActiveRecord::Relation [#<Score id: 13, name: "inagaki", subject: "English", score: 30, created_at: "2020-01-11 10:20:11", updated_at: "2020-01-11 10:20:11">, #<Score id: 33, name: "inagaki", subject: "English", score: 30, created_at: "2020-01-11 10:20:19", updated_at: "2020-01-11 10:20:19">, #<Score id: 37, name: "katori", subject: "English", score: 40, created_at: "2020-01-11 10:20:19", updated_at: "2020-01-11 10:20:19">, #<Score id: 17, name: "katori", subject: "English", score: 40, created_at: "2020-01-11 10:20:11", updated_at: "2020-01-11 10:20:11">, #<Score id: 9, name: "kusanagi", subject: "English", score: 50, created_at: "2020-01-11 10:20:11", updated_at: "2020-01-11 10:20:11">, #<Score id: 29, name: "kusanagi", subject: "English", score: 50, created_at: "2020-01-11 10:20:19", updated_at: "2020-01-11 10:20:19">, #<Score id: 25, name: "kimura", subject: "English", score: 55, created_at: "2020-01-11 10:20:19", updated_at: "2020-01-11 10:20:19">, #<Score id: 5, name: "kimura", subject: "English", score: 55, created_at: "2020-01-11 10:20:11", updated_at: "2020-01-11 10:20:11">, #<Score id: 28, name: "kimura", subject: "Music", score: 60, created_at: "2020-01-11 10:20:19", updated_at: "2020-01-11 10:20:19">, #<Score id: 8, name: "kimura", subject: "Music", score: 60, created_at: "2020-01-11 10:20:11", updated_at: "2020-01-11 10:20:11">]>
irb(main):037:0> 
```

# 11.小課題7
60点以上80点以下のレコードを得点の高い順に取得するコードを書いてください。

(ヒント)
ActiveRecord、where、不等号で検索してみましょう。

```
irb(main):039:0> Score.where('score BETWEEN ? and ? ', 60, 80).order(score:"DESC")
  Score Load (2.1ms)  SELECT  "scores".* FROM "scores" WHERE (score BETWEEN 60 and 80 ) ORDER BY "scores"."score" DESC LIMIT $1  [["LIMIT", 11]]
=> #<ActiveRecord::Relation [#<Score id: 18, name: "katori", subject: "Math", score: 80, created_at: "2020-01-11 10:20:11", updated_at: "2020-01-11 10:20:11">, #<Score id: 36, name: "inagaki", subject: "Music", score: 80, created_at: "2020-01-11 10:20:19", updated_at: "2020-01-11 10:20:19">, #<Score id: 16, name: "inagaki", subject: "Music", score: 80, created_at: "2020-01-11 10:20:11", updated_at: "2020-01-11 10:20:11">, #<Score id: 38, name: "katori", subject: "Math", score: 80, created_at: "2020-01-11 10:20:19", updated_at: "2020-01-11 10:20:19">, #<Score id: 27, name: "kimura", subject: "Science", score: 75, created_at: "2020-01-11 10:20:19", updated_at: "2020-01-11 10:20:19">, #<Score id: 32, name: "kusanagi", subject: "Music", score: 75, created_at: "2020-01-11 10:20:19", updated_at: "2020-01-11 10:20:19">, #<Score id: 12, name: "kusanagi", subject: "Music", score: 75, created_at: "2020-01-11 10:20:11", updated_at: "2020-01-11 10:20:11">, #<Score id: 20, name: "katori", subject: "Music", score: 75, created_at: "2020-01-11 10:20:11", updated_at: "2020-01-11 10:20:11">, #<Score id: 7, name: "kimura", subject: "Science", score: 75, created_at: "2020-01-11 10:20:11", updated_at: "2020-01-11 10:20:11">, #<Score id: 39, name: "katori", subject: "Science", score: 75, created_at: "2020-01-11 10:20:19", updated_at: "2020-01-11 10:20:19">, ...]>
```


# 12.小課題8
5人の名前がキーで、4科目の合計点数を値とするハッシュを取得するコードを書いてください。

```
irb(main):084:0> Score.group("name").sum("score")
   (3.7ms)  SELECT SUM("scores"."score") AS sum_score, "scores"."name" AS scores_name FROM "scores" GROUP BY "scores"."name"
=> {"katori"=>535, "kimura"=>510, "inagaki"=>580, "nakai"=>540, "kusanagi"=>600, "DIVE"=>90}
```
またそのコードを利用してkatoriさんの合計得点を取得するコードを書いてください。

```
irb(main):096:0> Score.where(name:"katori").group("name").sum("score")
   (2.2ms)  SELECT SUM("scores"."score") AS sum_score, "scores"."name" AS scores_name FROM "scores" WHERE "scores"."name" = $1 GROUP BY "scores"."name"  [["name", "katori"]]
=> {"katori"=>535}
```
(ヒント)
groupメソッドを使用してみましょう。



# 13.小課題9
名前がキーで、4科目の平均点を値とするハッシュで、平均点が70点以上のもののみを取得するコードを書いてください。

(ヒント)
ActiveRecord、group、havingで検索してみましょう。

```
irb(main):123:0> Score.group("name").having('avg(score) >=70').average("score")
   (2.5ms)  SELECT AVG("scores"."score") AS average_score, "scores"."name" AS scores_name FROM "scores" GROUP BY "scores"."name" HAVING (avg(score) >=70)
=> {"inagaki"=>0.725e2, "kusanagi"=>0.75e2, "DIVE"=>0.9e2}
```

# 14.小課題10
100,000のレコードが保存されているとします。
この場合allメソッドですべてのレコードを取得するとどうなってしまうでしょうか。
またその解決策として何が有効でしょうか。

(ヒント)
all、大量、処理、railsで検索してみましょう。


DBからデータを取得してきて処理をしたい場合、eachで処理しようとすると対象データがすべてメモリに展開されてしまいますが、find_eachは1行ずつメモリに展開するため、レコード数を気にせず処理をすることができます。


大量レコードに対して処理をするときはfind_eachやfind_in_batchesを使う