---
draft: true
slug:  ""
date:  2018-09-19T08:54:21+09:00
title: "Open Api Cheat Sheet"
categories:
- development
- business
tags:
isCJKLanguage: true
type: "post"
---

## リクエスト

```yml
paths:
  /users:
    post:
      parameters:
        - name:
          in:       (path|query)
          required: (true|false)
          schema: ~
      requestBody:
        content:
          application/json: #Media type
            schema: ~
      responses:
        # 省略
```

## レスポンス

```yml
paths:
  /users:
    post:
      parameters:
        # 省略
      responses:
        '200':                # HTTP status code
          description: OK
          content:
            application/json: # Media type
              schema: ~
```

## Schema（モデル）定義

定義側：

```yml
components:
  schemas:
    User:
      properties:
      id:
        type: integer
```

使う側：

```yml
# ~
  schema:
    $ref: '#/components/schemas/User'
```

- [Open API Spec](https://swagger.io/docs/specification/about/)
- [](https://swagger.io/specification/)
- [開発効率を上げる！Swaggerの記法まとめ](https://tech.starttoday-tech.com/entry/swagger_yaml)
