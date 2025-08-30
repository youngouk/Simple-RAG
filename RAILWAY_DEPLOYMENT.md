# Railway 배포 가이드

## 📋 배포 전 체크리스트

### ✅ 완료된 준비사항
- [x] GitHub에 최신 코드 푸시 완료
- [x] 로컬 테스트 완료 (95% 성공률)
- [x] 환경변수 처리 개선
- [x] Qdrant Cloud 연결 확인
- [x] Docker 설정 확인
- [x] 포트 설정 ($PORT 환경변수 사용)

## 🚀 Railway 배포 단계

### 1. Railway 프로젝트 접속
- URL: https://railway.app/project/48161f96-0d5a-48ec-ba45-41f15878bc54
- GitHub 저장소: https://github.com/youngouk/Simple-ragchat-pyver

### 2. 서비스 설정
```
Root Directory: backend
Start Command: uv run python main.py
Port: $PORT (자동 할당)
```

### 3. 필수 환경변수 설정
```bash
# 서버 설정
PORT=${{PORT}}
HOST=0.0.0.0
NODE_ENV=production

# Google API (필수)
GOOGLE_API_KEY=[YOUR_GOOGLE_API_KEY]

# Qdrant 벡터 데이터베이스 (필수)
QDRANT_URL=[YOUR_QDRANT_CLOUD_URL]
QDRANT_API_KEY=[YOUR_QDRANT_API_KEY]

# 추가 LLM API (선택사항)
ANTHROPIC_API_KEY=[YOUR_ANTHROPIC_API_KEY]
OPENAI_API_KEY=[YOUR_OPENAI_API_KEY]

# 리랭킹 (선택사항)
JINA_API_KEY=[YOUR_JINA_API_KEY]
COHERE_API_KEY=[YOUR_COHERE_API_KEY]
```

### 4. 배포 확인
배포 후 다음 엔드포인트들을 확인하세요:
- Health Check: `https://your-app.railway.app/health`
- API Info: `https://your-app.railway.app/api`
- System Stats: `https://your-app.railway.app/stats`

## ⚡ 성능 최적화 설정

### Docker 최적화
- Multi-stage 빌드 사용
- UV 패키지 매니저로 빠른 의존성 설치
- Non-root 사용자로 보안 강화

### 메모리 최적화
- 예상 메모리 사용량: ~700MB
- Railway 메모리 제한: 1GB 권장

### CPU 최적화
- 예상 CPU 사용률: 20-40%
- Railway vCPU: 1 vCPU 권장

## 🔍 배포 후 검증

### 1. 기본 기능 테스트
```bash
curl https://your-app.railway.app/health
curl https://your-app.railway.app/api
```

### 2. 채팅 기능 테스트
```bash
curl -X POST https://your-app.railway.app/api/chat \
  -H "Content-Type: application/json" \
  -d '{"message": "테스트 메시지", "session_id": "test-session"}'
```

### 3. 관리자 기능 테스트
```bash
curl https://your-app.railway.app/api/admin/status
```

## 🚨 문제 해결

### 일반적인 배포 오류
1. **환경변수 누락**: 위의 필수 환경변수들이 모두 설정되었는지 확인
2. **포트 오류**: PORT 환경변수가 Railway에서 자동 할당되는지 확인
3. **메모리 부족**: Railway 메모리 제한을 1GB로 설정
4. **의존성 오류**: pyproject.toml과 uv.lock 파일이 최신인지 확인

### 로그 확인
Railway 대시보드의 Logs 탭에서 배포 과정과 런타임 로그를 확인하세요.

## 📊 배포 성공 지표
- [ ] 배포 완료 (Build Success)
- [ ] 헬스 체크 통과 (Status: OK)
- [ ] 모든 모듈 정상 초기화
- [ ] API 응답 정상 (응답 시간 < 10초)
- [ ] 메모리 사용량 < 800MB
- [ ] CPU 사용률 < 50%

---

이 가이드를 따라 배포하면 Simple RAG Chatbot이 Railway에서 성공적으로 실행될 것입니다!