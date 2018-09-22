---
draft: true
slug:  "difference-btw-form-with-n-form-for"
date:  2018-09-03T17:54:41+09:00
title: "Form with と Form for の違い"
categories:
- development
tags:
- difference-btw
- rails
isCJKLanguage: true
---

fuga

<!--more-->

fuga

<!-- for_tag と for_for はもうすぐ廃止になる（[メモ]どこ情報？、https://github.com/rails/rails/issues/25197 / https://github.com/rails/rails/pull/26976/files、http://api.rubyonrails.org/classes/ActionView/Helpers/FormHelper.html#method-i-form_with）で今後 for_with に置き換わる。 -->

form_with モデル以外

```
<%= form_with url: users_path do |f| %>
  <%= form.text_field :email %>
  <%= form.submit %>
<% end %>
```

form_with モデル：

```
<%= form_with model @user do |form| %>
  <%= form.text_field :email %>
  <%= form.submit %>
<% end %>
```

`model` が設定された場合 `scope` と `url` は自動で追加されるので不要です。動作自体はform_for と似てる。

## 参照

- [Rails 5.1's form_with vs. form_tag vs. form_for](https://m.patrikonrails.com/rails-5-1s-form-with-vs-old-form-helpers-3a5f72a8c78a)
- [Using form_with vs form_for vs form_tag](https://www.engineyard.com/blog/using_form_with-vs-form_for-vs-form_tag)
