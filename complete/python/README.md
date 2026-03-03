# Python 앱 샘플

사용자가 데이터를 생성, 조회, 업데이트, 삭제할 수 있는 선박 운항 데이터 관리 서비스(Ship-ODMS)를 위한 완전한 FastAPI 백엔드 구현입니다.

## 🏗️ 아키텍처 개요

- **프레임워크**: Python 3.12+를 사용한 FastAPI
- **데이터베이스**: SQLite (`odms_api.db`)
- **API 문서화**: Swagger UI + OpenAPI 3.1 사양
- **CORS**: 교차 출처 요청 활성화
- **데이터 검증**: 포괄적인 검증을 포함한 Pydantic 모델

## 📁 프로젝트 구조

```text
python/
├── main.py              # FastAPI 애플리케이션 진입점
├── models.py            # Pydantic 데이터 모델 및 스키마
├── database.py          # SQLite 데이터베이스 작업
├── openapi.yaml         # OpenAPI 3.0.1 사양
├── odms_api.db          # SQLite 데이터베이스 파일 (자동 생성)
├── README.md           # 이 문서
└── .venv/              # 가상 환경 (설정 중 생성)
```

## 🚀 빠른 시작

### 전제 조건

준비를 위해 [README](../../README.md) 문서를 참조하세요.

### 1. 환경 설정

먼저 `$REPOSITORY_ROOT` 환경 변수를 설정하세요.

```bash
# bash/zsh
REPOSITORY_ROOT=$(git rev-parse --show-toplevel)
```

```powershell
# PowerShell
$REPOSITORY_ROOT = git rev-parse --show-toplevel
```

그런 다음 python 디렉터리로 이동하고 가상 환경을 생성하세요:

```bash
cd $REPOSITORY_ROOT/complete/python
```

가상 환경 생성

```bash
# uv 사용 (권장)
uv venv .venv
```

```bash
# 표준 Python 사용 (대안)
python -m venv .venv
```

### 2. 가상 환경 활성화

```bash
# Linux/macOS에서
source .venv/bin/activate
```

```bash
# Windows Command Prompt에서
.venv\Scripts\activate
```

### 3. 종속성 설치

```bash
# uv 사용 (권장)
uv pip install fastapi uvicorn python-multipart pyyaml
```

```bash
# pip 사용 (대안)
pip install fastapi uvicorn python-multipart pyyaml
```

### 4. OpenAPI 사양 복사

상위 디렉터리에서 OpenAPI 사양을 복사하세요.

```bash
# Linux/macOS에서
cp ../openapi.yaml .
```

```powershell
# Windows Command Prompt에서
xcopy ..\openapi.yaml .
```

### 5. 애플리케이션 실행

개발 서버 시작

```bash
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

애플리케이션은 다음에서 사용할 수 있습니다:

- **API 기본 URL**: `http://localhost:8000/api/`
- **Swagger UI**: `http://localhost:8000/docs`
- **OpenAPI 사양**: `http://localhost:8000/openapi.json`

## 📊 데이터베이스 스키마

애플리케이션은 다음 테이블을 포함한 SQLite를 사용합니다:

### Posts 테이블

- `id` (TEXT, PRIMARY KEY) - UUID
- `owner` (TEXT, NOT NULL) - 데이터 소유자
- `vessel` (TEXT, NOT NULL) - 선박 식별자
- `created_at` (TEXT, NOT NULL) - ISO 타임스탬프
- `updated_at` (TEXT, NOT NULL) - ISO 타임스탬프

## 🔌 API 엔드포인트

### 데이터

- `GET /api/posts` - 모든 데이터 목록
- `POST /api/posts` - 새 데이터 생성
- `GET /api/posts/{postId}` - 특정 데이터 가져오기
- `PATCH /api/posts/{postId}` - 데이터 업데이트
- `DELETE /api/posts/{postId}` - 데이터 삭제

## 🧪 API 테스트

### cURL 사용

#### 데이터 생성

```bash
curl -X POST "http://localhost:8000/api/posts" \
  -H "Content-Type: application/json" \
  -d '{"owner": "kr", "vessel": "imo1234567"}'
```

#### 모든 데이터 가져오기

```bash
curl -X GET "http://localhost:8000/api/posts"
```

### Swagger UI 사용

1. `http://localhost:8000/docs`로 이동
2. 모든 API 엔드포인트를 대화형으로 탐색하고 테스트
3. 요청/응답 스키마 및 예제 보기

## 📝 데이터 모델

### 요청 모델

- `NewPostRequest`: `{owner: str, vessel: str}`
- `UpdatePostRequest`: `{owner: str, vessel: str}`

### 응답 모델

- `Post`: 메타데이터와 개수를 포함한 전체 게시물 객체

## ⚙️ 구성

### 환경 변수

애플리케이션은 기본 설정을 사용하지만 사용자 정의할 수 있습니다:

- **데이터베이스**: SQLite 파일 `sns_api.db` (자동 생성)
- **호스트**: `0.0.0.0` (모든 인터페이스)
- **포트**: `8000`
- **CORS**: 모든 출처에 대해 활성화

### 프로덕션 고려사항

프로덕션 배포를 위해서는 다음을 고려하세요:

1. **데이터베이스**: PostgreSQL 또는 MySQL로 전환
2. **환경 변수**: 민감한 구성에 사용
3. **보안**: 인증 및 권한 추가
4. **CORS**: 특정 도메인으로 제한
5. **로깅**: 구조화된 로깅 구현
6. **모니터링**: 상태 확인 및 메트릭 추가

## 🛠️ 개발

### 파일 구성

- **`main.py`**: FastAPI 앱 구성, 미들웨어 및 라우트 정의
- **`models.py`**: 데이터 검증 및 직렬화를 위한 Pydantic 모델
- **`database.py`**: SQLite 작업, 연결 관리 및 CRUD 함수

### 코드 스타일

프로젝트는 다음을 따릅니다:

- Python PEP 8 스타일 가이드라인
- FastAPI 모범 사례
- 함수형 프로그래밍 패턴
- 전체적인 타입 힌트
- 포괄적인 오류 처리

### 새 기능 추가

1. `models.py`에서 Pydantic 모델 정의
2. `database.py`에서 데이터베이스 작업 추가
3. `main.py`에서 API 엔드포인트 생성
4. 필요한 경우 OpenAPI 사양 업데이트

## 🐛 문제 해결

### 일반적인 문제

1. **포트 이미 사용 중**: `--port 8001`로 포트 변경
2. **가상 환경 문제**: `rm -rf .venv && uv venv .venv`로 재생성
3. **데이터베이스 잠김**: 애플리케이션의 모든 실행 인스턴스 중지
4. **가져오기 오류**: 가상 환경이 활성화되어 있는지 확인

### 디버그 모드

추가 로깅과 함께 실행:

```bash
uvicorn main:app --host 0.0.0.0 --port 8000 --reload --log-level debug
```

## 📚 추가 리소스

- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [Pydantic Documentation](https://docs.pydantic.dev/)
- [SQLite Documentation](https://sqlite.org/docs.html)
- [OpenAPI Specification](https://swagger.io/specification/)

---

**면책조항**: 이 문서는 [GitHub Copilot](https://docs.github.com/copilot/about-github-copilot/what-is-github-copilot)에 의해 현지화되었습니다. 따라서 실수가 포함될 수 있습니다. 부적절하거나 잘못된 번역을 발견하면 [issue](https://github.com/kr-woojj/data-space/issues/new)를 생성해 주세요.
