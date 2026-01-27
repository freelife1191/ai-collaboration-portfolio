# 테스트 리포트
## [프로젝트명]

**프로젝트**: [프로젝트 ID/이름]
**최종 업데이트**: [YYYY-MM-DD]
**테스트 프레임워크**: [Jest/Vitest/Playwright 등] [버전]

---

## 📊 전체 테스트 현황

```
총 테스트 스위트: [N] passed, [N] total
총 테스트 케이스: [N] passed, [N] total
스냅샷: [N] total
실행 시간: [N]s
```

### 테스트 통과율

| 구분 | 통과 | 실패 | 스킵 | 통과율 |
|------|------|------|------|--------|
| Test Suites | [N] | [N] | [N] | [N]% ✅ |
| Test Cases | [N] | [N] | [N] | [N]% ✅ |

---

## 🎯 Day별 테스트 진행 현황

### Day 1: [단계명] (YYYY-MM-DD)

**테스트 대상**: [테스트 대상 설명]

| 테스트 스위트 | 테스트 케이스 | 결과 | 커버리지 |
|--------------|--------------|------|----------|
| `[파일명].spec.ts` | [테스트 케이스 설명] | ✅ PASS | [N]% |

**실행 명령어**:
```bash
[테스트 실행 명령어]
```

**결과**:
- Unit Test: ✅ [N] passed
- E2E Test: ✅ [N] passed
- 커버리지: [N]% ([세부 항목])

**검증 완료**: YYYY-MM-DD

---

### Day 2: [단계명] (YYYY-MM-DD)

**테스트 대상**: [테스트 대상 설명]

#### 1. [컴포넌트명] 단위 테스트

**파일**: `[경로]/[파일명].spec.ts`

| 테스트 케이스 | 설명 | 결과 | 실행 시간 |
|--------------|------|------|----------|
| [테스트 케이스 이름] | [설명] | ✅ PASS | < [N]ms |
| [테스트 케이스 이름] | [설명] | ✅ PASS | < [N]ms |

**커버리지**:
```
File              | % Stmts | % Branch | % Funcs | % Lines |
------------------|---------|----------|---------|---------|
[파일명]          |     [N] |      [N] |     [N] |     [N] |
```

**테스트 코드 예시**:
```typescript
it('[테스트 케이스 설명]', async () => {
  // Arrange
  const expected = [...];

  // Act
  const result = await service.method();

  // Assert
  expect(result).toEqual(expected);
});
```

---

### 전체 커버리지 리포트 (Day [N] 기준)

```
---------------------|---------|----------|---------|---------|-------------------
File                 | % Stmts | % Branch | % Funcs | % Lines | Uncovered Line #s
---------------------|---------|----------|---------|---------|-------------------
All files            |    [N]  |     [N]  |    [N]  |    [N]  |
 [디렉토리]          |    [N]  |     [N]  |    [N]  |    [N]  |
  [파일명]           |    [N]  |     [N]  |    [N]  |    [N]  | [라인 번호]
---------------------|---------|----------|---------|---------|-------------------
```

**해석**:
- ✅ **[컴포넌트 카테고리]**: [N]% 커버리지
- ⏳ **[컴포넌트 카테고리]**: [N]% ([이유])
- ❌ **[컴포넌트 카테고리]**: [N]% ([개선 필요])

**검증 완료**: YYYY-MM-DD

---

## 🔍 테스트 실행 방법

### 1. 단위 테스트 실행

```bash
# 모든 단위 테스트 실행
[테스트 명령어]

# Watch 모드로 실행
[Watch 모드 명령어]

# 특정 파일만 테스트
[특정 파일 테스트 명령어]
```

### 2. 커버리지 리포트 생성

```bash
# 커버리지 포함 테스트 실행
[커버리지 명령어]

# 커버리지 리포트 열기 (HTML)
[HTML 리포트 열기 명령어]
```

### 3. E2E 테스트 실행

```bash
# E2E 테스트 실행
[E2E 테스트 명령어]
```

---

## 📈 테스트 커버리지 목표

| 구분 | 현재 | 목표 (중기) | 최종 목표 |
|------|------|------------|-----------|
| **전체 커버리지** | [N]% | [N]% | 85%+ |
| **서비스 (Services)** | [N]% | [N]% | 90%+ |
| **컨트롤러 (Controllers)** | [N]% | [N]% | 85%+ |
| **유틸리티 (Utilities)** | [N]% | [N]% | 90%+ |

**참고**: [특정 파일 제외 이유 설명]

---

## 🧪 테스트 전략

### TDD (Test-Driven Development) 사이클

```
1. ❌ RED: 실패하는 테스트 작성
2. ✅ GREEN: 테스트를 통과하는 최소한의 코드 작성
3. 🔄 REFACTOR: 코드 개선 (테스트는 계속 통과)
```

### 테스트 피라미드

```
        /\
       /  \
      / E2E \         ← 소수 (Critical Path만)
     /------\
    /        \
   / Integration \    ← 중간 (API 엔드포인트)
  /------------\
 /              \
/   Unit Tests   \   ← 다수 (모든 Service, Utility)
------------------
```

**원칙**:
- Unit Test: 빠르고 많이 (80-90%)
- Integration Test: 중간 (10-15%)
- E2E Test: 느리고 적게 (5-10%)

---

## 📋 단계별 테스트 계획

### [Phase/Day N]: [단계명]

**추가 예정 테스트**:
- [ ] [컴포넌트명].[메서드명]() - [테스트 케이스 설명]
- [ ] [컴포넌트명].[메서드명]() - [테스트 케이스 설명]
- [ ] [Integration] - [통합 테스트 설명]
- [ ] [E2E] - [E2E 테스트 설명]

**예상 커버리지**: [N]%+

---

### [Phase/Day N+1]: [단계명]

**추가 예정 테스트**:
- [ ] [테스트 목록]

**예상 커버리지**: [N]%+

---

## 🐛 테스트 실패 이력

### YYYY-MM-DD: [테스트 실패 설명]

**문제**:
- [문제 설명]

**원인**:
- [원인 분석]

**해결 방법**:
```typescript
// Before (실패)
[실패했던 코드]

// After (성공)
[수정된 코드]
```

**검증**:
- [x] 테스트 재실행 → ✅ 통과
- [x] 회귀 테스트 추가

---

## 📚 참고 자료

### 테스트 프레임워크 공식 문서
- [Jest 테스트 작성](https://jestjs.io/docs/getting-started)
- [Vitest 가이드](https://vitest.dev/guide/)
- [Playwright E2E](https://playwright.dev/docs/intro)

### 프로젝트별 문서
- [프레임워크별 테스트 가이드 링크]
- [프로젝트 테스트 가이드 링크]

### 프로젝트 문서
- `docs/TEST-VERIFY.md` - 테스트 전략 및 검증 가이드
- `docs/LLD.md` - TDD 기반 개발 프로세스
- `docs/CHECKLIST.md` - 단계별 체크리스트

---

## ✅ 테스트 체크리스트

### [Phase/Day 1] 완료 항목

- [x] [테스트 항목 1]
- [x] [테스트 항목 2]
- [x] [테스트 항목 3]

### [Phase/Day 2] 예정 항목

- [ ] [테스트 항목 1]
- [ ] [테스트 항목 2]
- [ ] [테스트 항목 3]

### [Phase/Day N] 예정 항목

- [ ] [테스트 항목 1]
- [ ] [테스트 항목 2]
- [ ] [테스트 항목 3]

---

## 📊 테스트 메트릭 (선택 사항)

### 테스트 실행 시간 추이

| 날짜 | Unit Test | Integration Test | E2E Test | 전체 |
|------|-----------|------------------|----------|------|
| YYYY-MM-DD | [N]s | [N]s | [N]s | [N]s |
| YYYY-MM-DD | [N]s | [N]s | [N]s | [N]s |

### 커버리지 추이

| 날짜 | Statements | Branch | Functions | Lines |
|------|------------|--------|-----------|-------|
| YYYY-MM-DD | [N]% | [N]% | [N]% | [N]% |
| YYYY-MM-DD | [N]% | [N]% | [N]% | [N]% |

---

## 🎯 테스트 품질 기준

### 통과 기준
- ✅ 모든 Unit Test 100% 통과
- ✅ Integration Test 100% 통과
- ✅ E2E Test (Critical Path) 100% 통과
- ✅ 전체 커버리지 85%+ (목표 달성)

### 실패 허용 범위
- ❌ Flaky Test (불안정한 테스트) 허용 불가
- ❌ Skipped Test (스킵된 테스트) 이유 명시 필요
- ⚠️ Pending Test (보류 테스트) 최대 [N]개

---

## 💡 테스트 작성 가이드

### Good Practice ✅
```typescript
describe('UserService', () => {
  describe('register()', () => {
    it('should create a new user with hashed password', async () => {
      // Arrange
      const dto = { email: 'test@example.com', password: 'Test123!' };

      // Act
      const user = await service.register(dto);

      // Assert
      expect(user.email).toBe(dto.email);
      expect(user.passwordHash).not.toBe(dto.password);
      expect(bcrypt.compareSync(dto.password, user.passwordHash)).toBe(true);
    });
  });
});
```

### Bad Practice ❌
```typescript
// 너무 모호한 테스트
it('works', () => {
  expect(service.method()).toBeTruthy();
});

// 여러 개념을 한 번에 테스트
it('should do everything', () => {
  // 10개의 assertion...
});
```

---

**마지막 업데이트**: YYYY-MM-DD
**다음 업데이트 예정**: [Phase/Day N] 완료 후

---

## 📝 작성 가이드 (AI용)

이 템플릿을 사용할 때:

1. **[...]로 표시된 부분**을 프로젝트에 맞게 채워넣으세요
2. **Day별 섹션**은 개발 진행에 따라 추가하세요
3. **커버리지 리포트**는 실제 `pnpm test:cov` 결과를 복사하세요
4. **테스트 케이스 테이블**은 실제 spec 파일을 참고하여 작성하세요
5. **검증 완료** 날짜는 실제 테스트 통과 날짜를 기록하세요
6. **테스트 실패 이력**은 발생 시 즉시 기록하고 해결 방법을 문서화하세요
7. **참고 자료** 섹션은 프로젝트 기술 스택에 맞게 업데이트하세요

### 업데이트 주기
- 매 Day/Phase 완료 후 즉시 업데이트
- 테스트 추가/수정 시 해당 섹션 업데이트
- 주요 마일스톤 달성 시 전체 리뷰

### 연동 문서
- `CHECKLIST.md`: 테스트 검증 섹션에 TEST-REPORT.md 링크 추가
- `README.md`: 문서 목록에 TEST-REPORT.md 섹션 추가
- `TEST-VERIFY.md`: 테스트 전략 문서와 연계
