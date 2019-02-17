---
draft: true
slug:  "responding-with-json-in-rails"
date:  2018-09-18T20:30:06+09:00
title: "responding-with-json-in-rails"
categories:
- development
- business
tags:
isCJKLanguage: true
type: "post"
---

目的：
- Ruby on Rails（以下 RoR） でレスポンスを JSON 形式で返す

結論：
- Fast JSON API
  - 影響範囲が `/serializers/` 以下
  - ActiveRecord オブジェクトを操作するように操作できる
  - Active Model Serializer よりパフォーマンスに優れている

---

JSON シリアライズが必要な理由：
- サーバから返ってくるオブジェクトを JSON 形式にするため

JSON シリアライズライブラリ：
- **[Jbuilder](https://github.com/rails/jbuilder)**
- **[Active Model Serializer](https://github.com/rails-api/active_model_serializers) **
- **[Fast JSON API](https://github.com/Netflix/fast_jsonapi)**
- **[Jbuilder](https://github.com/rails/jbuilder)**

参照：
- [Responding with JSON in Rails](https://www.leighhalliday.com/responding-with-json-in-rails)
- [jbuilder vs rails-api/active_model_serializers for JSON handling in Rails 4](https://stackoverflow.com/questions/26097563/jbuilder-vs-rails-api-active-model-serializers-for-json-handling-in-rails-4#26557905)
