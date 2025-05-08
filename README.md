以下に、あなたの構成に最適化した **「VS Code Remote SSH + Spring Boot (Java) プロジェクト作成＆起動マニュアル」** を作成しました。
対象：**Windows A の VS Code から Windows B (WSL) に SSH で接続し、Java 開発を行う環境**

---

# 📘 VS Code Remote SSH × Spring Boot Java 開発手順書

## ✅ 構成前提

| 項目       | 内容                                                                |
| -------- | ----------------------------------------------------------------- |
| 開発PC     | Windows A（VS Codeインストール）                                          |
| 実行環境     | Windows B（WSL2）Java17 + Maven + Remote SSH対応済み                    |
| Java IDE | VS Code (Remote SSH 経由)                                           |
| Java     | OpenJDK 17（WSL 側にインストール済）                                         |
| Maven    | Apache Maven 3.9+（WSL 側にインストール済）                                  |
| 使用拡張     | Java Extension Pack／Spring Boot Extension Pack（VS Code 側にインストール済） |

---

## 🧩 ステップ①：Remote SSH 接続

1. VS Code 左下の「><」ボタン → `Remote-SSH: Connect to Host`
2. `~/.ssh/config` に定義された **Windows B**（WSL環境）を選択
3. 接続後、ターミナルで以下確認：

```bash
java -version    # openjdk 17 以上で OK
mvn -v           # Maven が動作すれば OK
```

---

## 🧪 ステップ②：Spring Boot プロジェクト作成（Spring Initializr）

1. `Ctrl + Shift + P` → コマンドパレットを開く
2. `Spring Initializr: Create a Maven Project` を選択
3. 対話式に以下を選ぶ：

| 設問           | 推奨選択                                       |
| ------------ | ------------------------------------------ |
| Language     | Java                                       |
| Packaging    | Jar                                        |
| Java version | 17                                         |
| Group        | `com.example`                              |
| Artifact     | `demo`                                     |
| Dependencies | Spring Web, Spring Boot DevTools, JPA など任意 |

4. プロジェクトの保存先には **Remote SSH 側の `/home/xxx/dev/` などを選択**
5. 自動的にプロジェクトが開く（開かない場合は `code .`）

---

## 🏗 ステップ③：Controller or static HTML を作成

### 🔹 A. 簡易 REST Controller（おすすめ）

```java
// src/main/java/com/example/demo/HelloController.java

package com.example.demo;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {
    @GetMapping("/")
    public String hello() {
        return "Hello, Spring Boot!";
    }
}
```

### 🔹 B. 静的ページで確認したい場合

```html
<!-- src/main/resources/static/index.html -->
<!DOCTYPE html>
<html>
<head><title>Spring Boot</title></head>
<body><h1>Hello from static HTML</h1></body>
</html>
```

---

## 🚀 ステップ④：アプリケーション起動

```bash
./mvnw spring-boot:run
```

または VS Code の **「Run」アイコン ▶ ボタン → Launch Spring Boot」** で起動。

---

## 🌐 ステップ⑤：動作確認

* ブラウザでアクセス：

  ```
  http://localhost:8080/
  ```
* `curl` でも可：

  ```bash
  curl http://localhost:8080/
  ```

---

## ⚙️ トラブルシュート

| 症状                                    | 対処                                                           |
| ------------------------------------- | ------------------------------------------------------------ |
| 404 Not Found (Whitelabel Error Page) | `@RestController` or `static/index.html` が無い／正しく配置されていない     |
| 8080 にアクセスできない                        | `application.properties` に `server.address=0.0.0.0` を追加して再起動 |
| Java Language Server が起動しない           | Java Extension Pack の再読み込み、`.vscode/settings.json` を確認       |

---

## 🔧 補足Tips

| 用途                    | 拡張機能 or Tips                              |
| --------------------- | ----------------------------------------- |
| REST API テスト          | Thunder Client／REST Client 拡張で Postman 不要 |
| 自動ビルド／Live reload     | `Spring DevTools` を dependencies に追加      |
| Spring Boot Dashboard | 拡張パックに含まれており VS Code 内から複数プロジェクト起動管理可能    |

---

## ✅ まとめ：できること

| 項目                 | できること                                                   |
| ------------------ | ------------------------------------------------------- |
| Java 未インストールでも     | Windows A に Java 不要、WSL 上だけで全て完結                        |
| Spring プロジェクトの作成   | Spring Initializr 対応、コマンド不要                             |
| Remote Debug / Dev | Remote SSH 経由で開発、デバッグ、ホットリロード                           |
| 拡張機能ベースで完結         | Java・Spring Boot 拡張により、Maven ビルド／実行／補完がすべて VS Code 内で完結 |

---

必要であればこの手順書を `README.dev.md` としてプロジェクト内に追加することもおすすめです 👍
ご希望があれば、PDF / Markdown ファイルでも出力できます！
