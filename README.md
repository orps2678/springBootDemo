# Spring Boot Demo

## 📌 專案簡介
本專案是一個基於 **Spring Boot 3.4.2** 的後端應用，使用 **Oracle** 作為資料庫，並搭配 **HikariCP** 進行連線池管理。專案內建 **JPA + Hibernate** 作為 ORM 框架，並支援 **Spring Security、Spring Batch、Actuator 監控**。

## 📂 技術棧
- **Spring Boot 3.4.2** - 應用框架
- **Spring Data JPA** - 資料持久化
- **Hibernate** - ORM 框架
- **Oracle 11g/12c** - 資料庫
- **HikariCP** - 高效能資料庫連線池
- **Spring Security** - 安全性控管
- **Spring Boot Actuator** - 監控與指標收集
- **Prometheus** - 指標監控 (透過 Micrometer 實現)
- **Lombok** - 簡化 Java 物件建構
- **JUnit & Spring Boot Test** - 單元測試

## ⚙️ 環境設置

### **1️⃣ 安裝必備工具**
請確保你已安裝以下工具：
- [JDK 17](https://adoptium.net/)
- [Maven 3.8+](https://maven.apache.org/)
- [Oracle Database 11g/12c](https://www.oracle.com/database/)
- [Git](https://git-scm.com/)

### **2️⃣ 設定環境變數 (可選)**
若不想在 `application.yml` 明文儲存資料庫密碼，請設定環境變數：
```sh
export DB_USERNAME=DEMO
export DB_PASSWORD=DEMO123456
```
Windows:
```sh
set DB_USERNAME=DEMO
set DB_PASSWORD=DEMO123456
```

### **3️⃣ 設定資料庫 (Oracle)**
請確保你的 Oracle 資料庫內 **存在 `XEPDB1`**，並執行以下 SQL 來建立 **專案用的帳號與 Schema**：
```sql
CREATE USER DEMO IDENTIFIED BY DEMO123456;
GRANT CONNECT, RESOURCE TO DEMO;
```

## 🚀 運行專案

### **1️⃣ 下載並建置專案**
```sh
git clone https://github.com/orps2678/springBootDemo.git
cd springBootDemo
mvn clean install
```

### **2️⃣ 啟動應用**
```sh
mvn spring-boot:run
```
或直接執行 `jar` 文件：
```sh
java -jar target/demo-0.0.1-SNAPSHOT.jar
```

## 🔍 API 測試
當應用啟動後，請確認應用運行在 **`http://localhost:8088`**，你可以測試：
```sh
curl -X GET http://localhost:8088/actuator/health
```

## 🔧 配置文件 (application.yml)
主要設定如下：
```yaml
server:
  port: 8088
  tomcat:
    threads:
      max: 200
    connection-timeout: 5000

spring:
  datasource:
    url: jdbc:oracle:thin:@localhost:1521/XEPDB1
    username: DEMO
    password: DEMO123456
    driver-class-name: oracle.jdbc.OracleDriver
    hikari:
      maximum-pool-size: 10
      minimum-idle: 2
      idle-timeout: 30000
  jpa:
    database-platform: org.hibernate.dialect.OracleDialect
    show-sql: true
    hibernate:
      ddl-auto: update

management:
  endpoints:
    web:
      exposure:
        include: health,info,metrics
```

## 🛠️ 主要依賴 (pom.xml)
```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
        <groupId>com.oracle.database.jdbc</groupId>
        <artifactId>ojdbc11</artifactId>
        <version>23.7.0.25.01</version>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>
</dependencies>
```

## 🛠️ 開發建議
- **`application.yml`** 密碼建議使用環境變數管理
- **JPA `ddl-auto` 設為 `update` 但生產環境應改為 `validate`**
- **監控：開啟 `/actuator` API，可整合 Prometheus**

## 📜 授權
本專案基於 [MIT License](https://opensource.org/licenses/MIT) 發布。

---

這份 `README.md` 讓專案的使用者快速理解如何運行與開發！🚀

