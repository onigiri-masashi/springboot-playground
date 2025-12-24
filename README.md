# Onigiri Boot 🍙

Spring Bootを使用したJava Webアプリケーションの雛形プロジェクトです。

## 技術スタック

- **Spring Boot**: 4.0.1
- **Java**: 25 (JDK 25)
- **ビルドツール**: Maven
- **テンプレートエンジン**: Thymeleaf
- **開発ツール**: Spring Boot DevTools

## 依存関係

- `spring-boot-starter-webmvc` - Web MVC機能
- `spring-boot-starter-thymeleaf` - Thymeleafテンプレートエンジン
- `spring-boot-devtools` - 開発時の自動リロード機能

## 前提条件

- JDK 21以上（推奨: JDK 25）
- Maven（Maven Wrapperが含まれているため、システムにインストール不要）

## プロジェクト構造

```
springboot-playground/
├── src/
│   ├── main/
│   │   ├── java/com/example/onigiriboot/
│   │   │   ├── OnigiriBootApplication.java    # メインアプリケーションクラス
│   │   │   └── HelloController.java           # サンプルコントローラー
│   │   └── resources/
│   │       ├── templates/
│   │       │   └── hello.html                 # サンプルビュー
│   │       ├── static/                         # 静的リソース（CSS, JS, 画像等）
│   │       └── application.properties          # アプリケーション設定
│   └── test/
│       └── java/com/example/onigiriboot/
│           └── OnigiriBootApplicationTests.java
├── pom.xml                                     # Maven設定ファイル
├── mvnw                                        # Maven Wrapper (Unix/Mac)
├── mvnw.cmd                                    # Maven Wrapper (Windows)
└── README.md                                   # このファイル
```

## セットアップ

### 1. プロジェクトのクローン

```bash
git clone <repository-url>
cd springboot-playground
```

### 2. 依存関係のダウンロード

```bash
./mvnw clean install
```

## アプリケーションの実行

### 開発モードで起動

```bash
./mvnw spring-boot:run
```

ブラウザで以下のURLにアクセス:
```
http://localhost:8080
```

### ビルドしてから実行

```bash
# ビルド
./mvnw clean package

# 実行
java -jar target/onigiri-boot-0.0.1-SNAPSHOT.jar
```

## 開発ガイド

### 新しいコントローラーの追加

`src/main/java/com/example/onigiriboot/` 配下に新しいコントローラークラスを作成:

```java
package com.example.onigiriboot;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class MyController {

    @GetMapping("/api/hello")
    @ResponseBody
    public String hello() {
        return "Hello from API!";
    }
}
```

### RESTful APIの作成

`@RestController`アノテーションを使用:

```java
package com.example.onigiriboot;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/api")
public class ApiController {

    @GetMapping("/data")
    public Map<String, String> getData() {
        return Map.of("message", "Hello, API!");
    }
}
```

### Thymeleafビューの追加

1. `src/main/resources/templates/` 配下にHTMLファイルを作成
2. コントローラーからビュー名を返す:

```java
@GetMapping("/mypage")
public String myPage(Model model) {
    model.addAttribute("title", "My Page");
    return "mypage";  // templates/mypage.html
}
```

### 静的リソース（CSS, JavaScript）の配置

以下のディレクトリに配置すると自動的に公開されます:

- `src/main/resources/static/` - CSS, JavaScript, 画像など
- `src/main/resources/public/` - 公開用リソース

例:
```
src/main/resources/static/css/style.css
→ http://localhost:8080/css/style.css
```

### アプリケーション設定

`src/main/resources/application.properties` で設定を変更:

```properties
# サーバーポート変更
server.port=8081

# ログレベル設定
logging.level.com.example.onigiriboot=DEBUG

# データベース接続（例）
# spring.datasource.url=jdbc:postgresql://localhost:5432/mydb
# spring.datasource.username=user
# spring.datasource.password=password
```

### テストの実行

```bash
# 全てのテストを実行
./mvnw test

# 特定のテストクラスを実行
./mvnw test -Dtest=OnigiriBootApplicationTests
```

## 主なMavenコマンド

```bash
# プロジェクトのクリーン
./mvnw clean

# コンパイル
./mvnw compile

# テスト実行
./mvnw test

# パッケージング（JARファイル作成）
./mvnw package

# テストをスキップしてビルド
./mvnw package -DskipTests

# 依存関係ツリーの表示
./mvnw dependency:tree

# Spring Bootアプリケーション起動
./mvnw spring-boot:run

# プロジェクト情報の表示
./mvnw help:effective-pom
```

## 開発時のTips

### 自動リロード（DevTools）

Spring Boot DevToolsが有効なため、コードを変更すると自動的に再起動されます。

### IDEでの開発

#### IntelliJ IDEA
1. `File` → `Open` → `pom.xml`を選択
2. 自動的にMavenプロジェクトとして認識されます

#### Eclipse
1. `File` → `Import` → `Existing Maven Projects`
2. プロジェクトディレクトリを選択

#### VS Code
1. Extension Packをインストール:
   - Java Extension Pack
   - Spring Boot Extension Pack
2. フォルダを開く

### デバッグモード

```bash
./mvnw spring-boot:run -Dspring-boot.run.jvmArguments="-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=5005"
```

IDEのリモートデバッガを`localhost:5005`に接続してデバッグできます。

### プロファイルの使用

開発、テスト、本番で設定を切り替える:

```bash
# application-dev.properties を使用
./mvnw spring-boot:run -Dspring-boot.run.profiles=dev

# application-prod.properties を使用
./mvnw spring-boot:run -Dspring-boot.run.profiles=prod
```

## トラブルシューティング

### ポートが既に使用されている

```bash
# ポート番号を変更して起動
./mvnw spring-boot:run -Dspring-boot.run.arguments=--server.port=8081
```

### キャッシュのクリア

```bash
./mvnw clean
rm -rf target/
./mvnw compile
```

### 依存関係の更新

```bash
./mvnw clean install -U
```

## ライセンス

このプロジェクトはサンプルプロジェクトです。

## 参考資料

- [Spring Boot公式ドキュメント](https://spring.io/projects/spring-boot)
- [Thymeleafドキュメント](https://www.thymeleaf.org/documentation.html)
- [Mavenガイド](https://maven.apache.org/guides/)
