# RAG Chatbot Backend

FastAPI 기반 한국어 RAG 챗봇 백엔드 서비스

## Features

- 다중 LLM 지원 (Google Gemini, OpenAI, Anthropic)
- Qdrant 벡터 데이터베이스
- 하이브리드 검색 (Dense + Sparse)
- 문서 업로드 및 처리
- 세션 기반 대화 관리

## API Endpoints

- `/health` - 헬스 체크
- `/api/chat` - 채팅 API
- `/api/upload` - 문서 업로드
- `/docs` - API 문서

## Environment Variables

See `.env.example` for required environment variables.