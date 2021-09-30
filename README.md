# DMM WEBCAMPコンテンツ 課題1【Bookers1を完成させよう(デバッグ形式)】
DMM WEBCAMPの学習コンテンツ　応用課題の研修課題です。
## 修正内容
サーバー起動時
1. You’re on Rails!が表示されてしまう
You’re on Rails!が表示される原因はrootの指定がされていないためです。
routes.rbに指定しましょう。

homes/top
2. LoadError
routes.rbでrootの指定をしたhomes_controller.rbを読み込もうとしているのですが、
読み込めずにエラー。
homes_controller.rbのCass名をHomeに修正



3. startのリンクをクリックしても反応しない
startのリンクであるtop.html.erbのリンクを、<%= link_to "start", books_path %>に修正

books/index
4. NameError undefined local variable or method `book'
render先である_form.html.erbでbookが未定義になっています。
books/index.html.erbのrenderを<%= render 'books/form', book: @book %>に修正


5. NoMethodError undefined method 'title'
titleがundefined methodとエラーが出ているため、schema.rbを確認。
db/migrateファイルに（t.string :title）カラムを追加


6. 空白で投稿するとエラーが出る
eachはメソッドであるため未定義であるというわけではありません。
eachの前である@booksが未定義になっています。
books_controller.rbのcrerateに（@books = Book.all）を追加。


7. 5の解決後、再度一覧画面を表示するとNo route matches
エラー文にmissing required keys: [:id]と書かれています。
urlにidを要求しているのに渡せていない状況になりますので
link_toに　<%= link_to 'Edit', edit_book_path(book.id), class: "edit_#{book.id}" %>
を追加。


books/edit
8. 編集フォームのupdateボタンが発火しない
edit.html.erbのフォーム近辺に余計な記述がないかを確認しましょう。
#<form> ←余計な記述
<%= render 'books/form', book: @book %>
#</fome>


9. 投稿の編集が行えない
updateメソッドはsaveメソッドと違い、「何をどう更新するか」を指定するために、 @book.update(...)と、後ろに引数が必要になります。
books_controller.rbに（@book.update(book_params)　を追加
