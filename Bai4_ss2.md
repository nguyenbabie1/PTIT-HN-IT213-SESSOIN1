# BÀI 4: CẤU HÌNH HYBRID AI RUNTIME

Cấu hình dưới đây sử dụng **Spring Boot 4.0.0 và Spring AI 2.0.0**. Spring AI 2.0 hỗ trợ Spring Boot 4.0.x–4.1.x và cung cấp BOM để đồng bộ phiên bản dependency. [Spring AI Getting Started](https://docs.spring.io/spring-ai/reference/getting-started.html).

## 1. File `build.gradle`

```groovy
plugins {
    id 'java'
    id 'org.springframework.boot' version '4.0.0'
}

group = 'com.rikkei'
version = '0.0.1-SNAPSHOT'

java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(17)
    }
}

repositories {
    mavenCentral()
}

dependencies {
    // Spring AI BOM quản lý đồng bộ phiên bản các module Spring AI
    implementation platform(
            'org.springframework.ai:spring-ai-bom:2.0.0'
    )

    // Xây dựng REST API
    implementation 'org.springframework.boot:spring-boot-starter-web'

    // ChatModel chạy local thông qua Ollama
    implementation 'org.springframework.ai:spring-ai-starter-model-ollama'

    // OpenAI-compatible client dùng để kết nối OpenRouter
    implementation 'org.springframework.ai:spring-ai-starter-model-openai'

    // Testing
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    testRuntimeOnly 'org.junit.platform:junit-platform-launcher'
}

tasks.named('test') {
    useJUnitPlatform()
}
```

Hai starter chính:

```groovy
implementation 'org.springframework.ai:spring-ai-starter-model-ollama'
implementation 'org.springframework.ai:spring-ai-starter-model-openai'
```

Starter OpenAI có thể kết nối OpenRouter vì OpenRouter cung cấp API tương thích chuẩn OpenAI. Các artifact trên là tên starter chính thức của Spring AI hiện hành. [Ollama Chat](https://docs.spring.io/spring-ai/reference/api/chat/ollama-chat.html), [OpenAI Chat](https://docs.spring.io/spring-ai/reference/api/chat/openai-chat.html).

---

## 2. File `application-local.properties`

Vị trí:

```text
src/main/resources/application-local.properties
```

Nội dung:

```properties
# Chỉ kích hoạt OllamaChatModel trong profile local
spring.ai.model.chat=ollama

# Địa chỉ Ollama local
spring.ai.ollama.base-url=http://localhost:11434

# Model được Ollama sử dụng
spring.ai.ollama.chat.model=qwen2.5-coder:7b

# Cấu hình tùy chọn
spring.ai.ollama.chat.temperature=0.2
```

Trước khi chạy ứng dụng, cần bảo đảm Ollama đang hoạt động và model đã được tải:

```bash
ollama pull qwen2.5-coder:7b
```

Kiểm tra model:

```bash
ollama list
```

---

## 3. File `application-cloud.properties`

Vị trí:

```text
src/main/resources/application-cloud.properties
```

Nội dung:

```properties
# Chỉ kích hoạt OpenAiChatModel trong profile cloud
spring.ai.model.chat=openai

# OpenRouter cung cấp API tương thích OpenAI
spring.ai.openai.base-url=https://openrouter.ai/api/v1

# Đọc API key từ biến môi trường, không ghi cứng trong source code
spring.ai.openai.api-key=${OPENROUTER_API_KEY}

# Model trên OpenRouter
spring.ai.openai.chat.model=google/gemini-2.5-flash

# Cấu hình tùy chọn
spring.ai.openai.chat.temperature=0.2
```

OpenRouter sử dụng endpoint chuẩn:

```text
https://openrouter.ai/api/v1/chat/completions
```

và xác thực bằng Bearer API key. [OpenRouter Quickstart](https://openrouter.ai/docs/quickstart).

---

## 4. File `application.properties`

Vị trí:

```text
src/main/resources/application.properties
```

Nội dung:

```properties
# Profile mặc định của ứng dụng
spring.profiles.active=local

# Dự án hiện chỉ sử dụng ChatModel
# Tắt các loại model khác để tránh auto-configuration không cần thiết
spring.ai.model.embedding=none
spring.ai.model.image=none
spring.ai.model.audio.speech=none
spring.ai.model.audio.transcription=none
spring.ai.model.moderation=none
```

Do profile mặc định là `local`, chạy lệnh sau sẽ sử dụng Ollama:

```bash
./gradlew bootRun
```

Trên Windows:

```powershell
.\gradlew.bat bootRun
```

---

## 5. Chạy ứng dụng với profile `cloud`

### Windows PowerShell

Thiết lập API key:

```powershell
$env:OPENROUTER_API_KEY="sk-or-v1-your-api-key"
```

Chạy qua Gradle:

```powershell
.\gradlew.bat bootRun --args="--spring.profiles.active=cloud"
```

### Linux hoặc macOS

```bash
export OPENROUTER_API_KEY="sk-or-v1-your-api-key"
./gradlew bootRun --args='--spring.profiles.active=cloud'
```

### Chạy file JAR

Build ứng dụng:

```bash
./gradlew clean bootJar
```

Chạy bằng profile `cloud`:

```bash
java -jar build/libs/hybrid-ai-0.0.1-SNAPSHOT.jar \
  --spring.profiles.active=cloud
```

Biến môi trường cũng có thể dùng để chọn profile:

```bash
SPRING_PROFILES_ACTIVE=cloud \
OPENROUTER_API_KEY="sk-or-v1-your-api-key" \
java -jar build/libs/hybrid-ai-0.0.1-SNAPSHOT.jar
```

## Lưu ý về phiên bản

Spring AI 2.0 sử dụng:

```properties
spring.ai.ollama.chat.model=...
spring.ai.openai.chat.model=...
```

Không nên trộn với cú pháp của một số phiên bản cũ:

```properties
spring.ai.ollama.chat.options.model=...
spring.ai.openai.chat.options.model=...
```

Quan trọng nhất là `build.gradle` và các property phải cùng thuộc một phiên bản Spring AI.


# Bài 4: Cấu hình Hybrid AI Runtime

Cấu hình dưới đây sử dụng Spring Boot 4.0.0 và Spring AI 2.0.0.

## 1. File `build.gradle`

```groovy
plugins {
    id 'java'
    id 'org.springframework.boot' version '4.0.0'
}

group = 'com.rikkei'
version = '0.0.1-SNAPSHOT'

java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(17)
    }
}

repositories {
    mavenCentral()
}

dependencies {
    // Spring AI BOM: quản lý đồng bộ phiên bản Spring AI
    implementation platform(
            'org.springframework.ai:spring-ai-bom:2.0.0'
    )

    // Xây dựng REST API
    implementation 'org.springframework.boot:spring-boot-starter-web'

    // Kết nối Ollama local
    implementation 'org.springframework.ai:spring-ai-starter-model-ollama'

    // Kết nối OpenRouter qua chuẩn OpenAI-compatible
    implementation 'org.springframework.ai:spring-ai-starter-model-openai'

    // Testing
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    testRuntimeOnly 'org.junit.platform:junit-platform-launcher'
}

tasks.named('test') {
    useJUnitPlatform()
}
```

Spring AI cung cấp BOM để đồng bộ phiên bản giữa các starter. Hai dependency chính thức là `spring-ai-starter-model-ollama` và `spring-ai-starter-model-openai`. [Spring AI Documentation](https://docs.spring.io/spring-ai/reference/getting-started.html).

## 2. File `application-local.properties`

Đường dẫn:

```text
src/main/resources/application-local.properties
```

Nội dung:

```properties
# Chỉ kích hoạt OllamaChatModel
spring.ai.model.chat=ollama

# Địa chỉ Ollama local
spring.ai.ollama.base-url=http://localhost:11434

# Model sử dụng
spring.ai.ollama.chat.model=qwen2.5-coder:7b

# Cấu hình tùy chọn
spring.ai.ollama.chat.temperature=0.2
```

Chuẩn bị model trước khi chạy:

```bash
ollama pull qwen2.5-coder:7b
```

## 3. File `application-cloud.properties`

Đường dẫn:

```text
src/main/resources/application-cloud.properties
```

Nội dung:

```properties
# Chỉ kích hoạt OpenAiChatModel
spring.ai.model.chat=openai

# OpenRouter hỗ trợ chuẩn OpenAI-compatible
spring.ai.openai.base-url=https://openrouter.ai/api/v1

# Lấy API key từ biến môi trường
spring.ai.openai.api-key=${OPENROUTER_API_KEY}

# Model được gọi qua OpenRouter
spring.ai.openai.chat.model=google/gemini-2.5-flash

# Cấu hình tùy chọn
spring.ai.openai.chat.temperature=0.2
```

API key không được ghi trực tiếp vào file properties hoặc đưa lên Git.

## 4. File `application.properties`

Đường dẫn:

```text
src/main/resources/application.properties
```

Nội dung:

```properties
# Profile mặc định
spring.profiles.active=local

# Dự án chỉ sử dụng chức năng ChatModel
spring.ai.model.embedding=none
spring.ai.model.image=none
spring.ai.model.audio.speech=none
spring.ai.model.audio.transcription=none
spring.ai.model.moderation=none
```

Các cấu hình `none` giúp ngăn Spring AI tự động tạo những model không được sử dụng.

## 5. Chạy profile `cloud` từ CLI

### Windows PowerShell

```powershell
$env:OPENROUTER_API_KEY="your-openrouter-api-key"
.\gradlew.bat bootRun --args="--spring.profiles.active=cloud"
```

### Linux hoặc macOS

```bash
export OPENROUTER_API_KEY="your-openrouter-api-key"
./gradlew bootRun --args='--spring.profiles.active=cloud'
```

### Chạy bằng file JAR

Build ứng dụng:

```bash
./gradlew clean bootJar
```

Chạy profile Cloud:

```bash
java -jar build/libs/hybrid-ai-0.0.1-SNAPSHOT.jar \
  --spring.profiles.active=cloud
```

Tham số CLI `--spring.profiles.active=cloud` có độ ưu tiên cao hơn giá trị `local` trong `application.properties`, vì vậy ứng dụng sẽ nạp `application-cloud.properties` mà không cần sửa mã Java.
