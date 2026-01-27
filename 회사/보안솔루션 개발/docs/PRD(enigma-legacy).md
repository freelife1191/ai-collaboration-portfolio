# Enigma 데이터 암복호화 Legacy(Rust 1.63) 버전 PRD 문서

---

## 1. 프로젝트 코딩 스타일 가이드라인

### 1. 일반 원칙
- 일관성을 유지한다
- 명확하고 이해하기 쉬운 코드를 작성한다
- 중복을 피한다 (DRY - Don't Repeat Yourself)

### 2. 주석
- 복잡하거나 이해하기 어려운 로직에는 주석을 작성한다
- `// TODO:` 주석을 사용하여 향후 수정해야 할 부분을 표시한다

### 3. 에러 처리
- 예상되는 오류는 `try-catch` 블록 등을 사용하여 적절히 처리한다
- 사용자에게 명확한 오류 메시지를 제공한다
- 민감한 정보가 오류 메시지에 포함되지 않도록 주의한다

### 4. 제약조건

- 반드시 Rust 1.63 이하의 버전에서 동작하는 패키지들만을 사용해서 개발해야 한다
- Spec 과 기능 요구 사항을 반드시 준수할 것
- enigma-rust-legacy 경로에서만 개발할 것
- enigma-rust-legacy/src 내부에 생성된 파일 구조를 유지하면서 개발하고 절대로 파일을 추가로 생성하지 말것

---

## 2. Spec

- Rust Version: 1.63
- `AES128`, `AES192`, `AES256` 양방향 암호화 지원
  - `AES-CBC-PKCS7`로 암호화
- `SHA256`, `SHA512`, `SHA384`, `SHA512_256` 단방향 해쉬 지원

## 3. 프로젝트 구조

- `src/domain`: Enigma 암복호화 도메인 객체
  - enigma_cipher_spec.rs: config.json 의 credential을 복호화하여 사용하는 암복호화 설정 객체
  - enigma_config.rs: config.json 파일을 읽어 EnigmaSession을 생성하기 위한 설정 객체
- `src/error`: Enigma 암복호화 에러 객체
  - enigma_error.rs: Enigma 암복호화 에러 객체
  - enigma_error_test.rs: Enigma 암복호화 에러 테스트
- `src/kms`: AWS KMS 서비스
  - aws_kms_service.rs: seed를 암복호화 하기 위한 AWS KMS 서비스 객체
  - aws_kms_service_test.rs: AWS KMS 서비스 테스트
- `src/resources`: EnigmaSession 을 생성하기 위한 config.json 설정 파일이 위치하는 경로
- `src/session`: 사용자에게 제공되는 Enigma 암복호화 세션 객체
  - enigma_session.rs: 사용자에게 제공하는 EnigmaSession 객체 생성 및 암복호화 Function
  - enigma_session_test.rs: EnigmaSession 테스트
- `src/util`: Enigma 암복호화 유틸리티 객체
  - enigma_util.rs: 데이터를 AES 암복호화 및 SHA 해쉬 처리하는 Function
  - enigma_util_test.rs: Enigma 암복호화 유틸리티 테스트

---

## 4. 개발 순서

- 반드시 아래의 순서대로 개발을 진행한다
- 단계별로 개발이 완료되면 테스트 코드를 작성하고 테스트 코드가 통과할 때까지 해당 단계를 개발하고 테스트 하는 것을 반복한다
- 테스트 코드는 반드시 해당 패키지의 `*_test.rs` 에만 작성한다

### Part 1. Enigma 암복호화 유틸리티 기능 개발

`src/util` 패키지에 있는 `enigma_util.rs` 파일에 단계별로 기능을 구현한다

1. length 길이를 받아 Random한 byte array 데이터를 생성하는 기능을 구현한다
2. SHA-256, MD5 파라메터, byte array 데이터, count 를 인자값으로 받아 데이터를 count 만큼 digest 하는 기능을 구현한다
3. 데이터를 Key 길이에 따라 AES128, 192, 256 알고리즘으로 암복호화하는 기능을 구현한다
4. byte array 데이터를 SHA256, SHA512, SHA384, SHA512_256을 선택해 해쉬 처리 하는 기능을 구현한다
5. byte array 데이터를 base64 인코딩하고 인코딩된 base64 데이터를 디코딩하는 기능을 구현한다
6. json 데이터를 객체에 Parsing 해서 담아주는 Generic 함수를 구현한다
7. 특정 경로에 있는 json 파일을 읽어서 특정 객체에 Parsing 하는 Generic 함수를 구현한다

### Part 2. AWS KMS 서비스 기능 개발

`src/kms` 패키지에 있는 `aws_kms_service.rs` 파일에 단계별로 기능을 구현한다

Rust 1.63 버전에서는 버전이 낮아 라이브러리가 잘 지원되지 않는 문제로 AWS KMS를 직접 통신하도록 구현해야 한다

사용에 필요한 라이브러리 가 있다면 의존성에 문제가 없는지 확인 요청 후 승인할때만 추가한다

1. `src/util/enigma_util.rs` 에서 `parse_json_path` 을 사용해서 `src/resources/config.json` 파일을 읽어 `AwsConfig` 객체를 생성하는 기능을 구현한다

```rust
pub struct AwsConfig {
    aws_kms_key_arn: String,
    aws_access_key_id: String,
    aws_secret_access_key: String,
}
```
2. AWS Config 정보로 AWS KMS 와 통신하는 Client 코드를 구현한다
3. 현재까지 개발된 코드를 참고해서 AWS KMS로 byte array 데이터를 암복호화 하는 기능을 구현한다

---

## 5. 기능 요구 사항 (Functional Requirements)

아래의 기능들이 필수적으로 개발되어야 함

### 1. 암복호화 라이브러리 사용을 위한 Config 설정 기능
- 특정 경로에 있는 `config.json` 파일을 로딩해 암복호화 라이브러리를 설정
  - `src/resources/config.json` 파일 참고

### 2. 암복호화 라이브러리 사용을 위한 Config 생성 기능

고객에게 제공하기 위해 아래의 구조로 Config 파일을 생성할 수 있는 기능이 필요함

```json
{
  "aws_kms_key_arn": "arn:aws:kms:ap-northeast-2:11111111111:key/XXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX",
  "aws_access_key_id": "AXXXXXXXXXXX",
  "aws_secret_access_key": "7XXXXXXXXXXXXXXXXXXXXX",
  "seed": "AXXXXXXXXXXXXXXXXXXX",
  "credential": "MXXXXXXXXXXXXXXXXXXXXXXXc="
}
```

### 3. 양방향 암호화
- Plaintext 를 KEY 길이에 따라 유연하게 `AES128`, `AES192`, `AES256` 알고리즘으로 암호화
- AES 알고리즘으로 암호화된 데이터를 복호화

enigma-lib의 암호화 테스트 코드


#### 암호화 테스트

- `EnigmaSession::of_byte` 로 `config.json` 파일을 읽어 `EnigmaSession` 객체를 생성
- `EnigmaSession::encrypt_id` 로 암호화 테스트

```rust
#[test]
fn enigma_session_encrypt_test() -> Result<(), String> {
  let path_buf = path::absolute("src/resources/config.json").expect("Unable to get absolute path");
  let path = path_buf.as_path();
  let bytes = read_file_as_bytes(path).unwrap();
  let session = match EnigmaSession::of_byte(bytes.as_slice()) {
    Ok(session) => session,
    Err(e) => return Err(e.to_string()),
  };
  let encrypted = session.encrypt_id("test".to_string(), 100)
          .map_err(|e| format!("{:?}", e))?;
  println!("Encrypted: {:?}", encrypted);
  Ok(())
}
```

#### 복호화 테스트

- `EnigmaSession::of_byte` 로 `config.json` 파일을 읽어 `EnigmaSession` 객체를 생성
- `EnigmaSession::decrypt_id` 로 복호화 테스트

```rust
#[test]
fn enigma_session_decrypt_test() -> Result<(), String> {
  let path_buf = path::absolute("src/resources/config.json").expect("Unable to get absolute path");
  let path = path_buf.as_path();
  let bytes = read_file_as_bytes(path).unwrap();
  let session = match EnigmaSession::of_byte(bytes.as_slice()) {
    Ok(session) => session,
    Err(e) => return Err(e.to_string()),
  };
  let decrypted = session.decrypt_id(encrypted, 100)
          .map_err(|e| format!("{:?}", e))?;
  println!("Decrypted: {:?}", decrypted);
  Ok(())
}
```

#### 해쉬 테스트

- `EnigmaSession::of_byte` 로 `config.json` 파일을 읽어 `EnigmaSession` 객체를 생성
- `EnigmaSession::hash_id` 로 해쉬 테스트

```rust
#[test]
fn enigma_session_hashed_test() -> Result<(), String> {
  let path_buf = path::absolute("src/resources/config.json").expect("Unable to get absolute path");
  let path = path_buf.as_path();
  let bytes = read_file_as_bytes(path).unwrap();
  let session = match EnigmaSession::of_byte(bytes.as_slice()) {
    Ok(session) => session,
    Err(e) => return Err(e.to_string()),
  };
  let hashed = session.encrypt_id("we are the champion".to_string(), 400)
          .map_err(|e| format!("{:?}", e))?;
  println!("Hashed: {:?}", hashed);
  Ok(())
}
```


### 4. 단방향 해쉬
- Plaintext 를 `SHA256`, `SHA512`, `SHA384`, `SHA512_256` 단방향 해쉬
- HASH

---

## 6. 설정 파일(config.json) 구조

### `config.json`
- `aws_kms_key_arn`: AWS KMS Key ARN
- `aws_access_key_id`: AWS Access Key ID
- `aws_secret_access_key`: AWS Secret Access Key
- `seed`:
  - `credential` 값을 암호화하기 위한 Credential Key, Credential IV를 생성하기 위한 랜덤으로 생성된 16 Byte 크기의 값
  - config 파일로 제공시 KMS로 암호화하고 BASE64로 인코딩해서 제공
- `credential`: 암복호화와 해쉬 처리를 하기 위한 알고리즘 설정 값

#### `Credential Key`, `Credential IV` 구조

- `credential` 값을 암복호화 하기위한 Key, IV 값 
- `config.json` 파일에 있는 `seed` 값은 KMS로 암호화된 값으로 제공되며 `config.json`의 `seed` 값을 사용시 KMS로 복호화 후 사용

- `Credential Key`: seed 값과 SHA-256 해쉬 알고리즘으로 10번 digest
- `Credential IV`: seed 값과 MD5 해쉬 알고리즘으로 10번 digest

#### `credential` 구조

아래의 데이터를 Json Array 구조에 담아서 byte array로 변환 후 `Credential Key`, `Credential IV`값과 AES 알고리즘 암호화해서 `credential` 값으로 제공

- `id`: 100: 양방향 암호화 알고리즘 ID, 400: 해쉬 알고리즘 ID
- `ag`: 암호화 OR 해쉬 알고리즘 (id가 100이면 양방향 암호화 알고리즘 AES, 400이면 해쉬 알고리즘 default SHA-512)
- `bm`: 블록 모드 ((id가 100이면 CBC, 400이면 None)
- `pm`: 패딩 모드 (id가 100이면 PKCS7, 400이면 None)
- `of`: 출력 포맷(b64(default): BASE64, h16: HEX)
- `ky`: 암호화 키 (Hex Byte Array)
- `iv`: 초기화 벡터 (Hex Byte Array)
- `no`: nonce (None)
- `ad`: 추가 데이터 (None)

```json
[
  {
    "id": 100,
    "ag": "AES",
    "bm": "CBC",
    "pm": "PKCS7",
    "of": "b64",
    "ky": "fXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX=",
    "iv": "AAAAAAAAAAAAAAAAAAAAAA==",
    "no": null,
    "ad": null
  },
  {
    "id": 400,
    "ag": "SHA-512",
    "bm": null,
    "pm": null,
    "of": "b64",
    "ky": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX==",
    "iv": null,
    "no": null,
    "ad": null
  }
]
```