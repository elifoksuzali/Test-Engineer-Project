# Test Engineer Project — Gradle Çok Modüllü Java Uygulaması

**Gradle** ile kurulmuş, çok modüllü (multi-project) örnek bir **Java** uygulamasıdır.
Bir metni alır; kelimelerine ayırır, yeniden birleştirir ve baş harflerini büyüterek ekrana yazar.
Projenin amacı Gradle çok modüllü yapı, modüller arası bağımlılık, birim testleri (JUnit) ve
sürekli entegrasyon (CI) kurulumunu uygulamalı göstermektir.

---

## 🧱 Modüller

Proje üç modül + ortak yapılandırmadan oluşur (`settings.gradle` → `include('app', 'list', 'utilities')`):

| Modül | İçerik | Görevi |
|-------|--------|--------|
| **`app`** | `App`, `MessageUtils` | Giriş noktası; mesajı alır, böler, birleştirir, büyük harfe çevirip yazar |
| **`list`** | `LinkedList` | Uygulamanın kullandığı basit bağlı liste veri yapısı |
| **`utilities`** | `StringUtils`, `SplitUtils`, `JoinUtils` | Metin bölme/birleştirme yardımcıları |
| **`buildSrc`** | convention `.gradle` dosyaları | Modüller arası ortak Gradle yapılandırması (common / library / application) |

Bağımlılık akışı: **app → utilities → list**. `app` ayrıca `org.apache.commons.text` kullanır.

Örnek çalışma: `"Hello      World!"` → böl → birleştir → `Hello World` (baş harfler büyük).

---

## 📁 Proje Yapısı

```
.
├── app/
│   └── src/
│       ├── main/java/projectSoftware/app/{App,MessageUtils}.java
│       └── test/java/projectSoftware/app/MessageUtilsTest.java
├── list/
│   └── src/
│       ├── main/java/projectSoftware/list/LinkedList.java
│       └── test/java/projectSoftware/list/LinkedListTest.java
├── utilities/
│   └── src/main/java/projectSoftware/utilities/{StringUtils,SplitUtils,JoinUtils}.java
├── buildSrc/                 # ortak Gradle convention eklentileri
├── gradle/wrapper/           # Gradle wrapper
├── gradlew / gradlew.bat     # Gradle wrapper çalıştırıcıları
├── settings.gradle
├── .travis.yml               # CI yapılandırması (Travis)
└── README.md
```

---

## 🛠️ Gereksinimler

- **JDK 14+** (CI `oraclejdk14` ile derler)
- Gradle kurmanıza gerek yok — depo **Gradle Wrapper** (`gradlew`) içerir.

---

## 🚀 Derleme, Test ve Çalıştırma

```bash
# Derle
./gradlew build          # Windows: gradlew.bat build

# Testleri çalıştır (JUnit)
./gradlew test

# Uygulamayı çalıştır
./gradlew :app:run
```

---

## 🔄 Sürekli Entegrasyon (CI)

`.travis.yml` ile her push'ta Travis CI üzerinde `./gradlew build --scan -s` çalıştırılır.

---

