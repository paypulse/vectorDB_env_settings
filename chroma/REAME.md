# Chorma DB 

# Chroma DB – Postman 사용 가이드 (Docker 기준)

이 문서는 **Docker로 실행 중인 Chroma DB**를 대상으로 **Postman만 사용해서**
Collection 생성 → 문서 추가 → 벡터 검색까지 수행하는 최소 실습 가이드입니다.

> 목적:
>
> * 코드 없이 Chroma DB API 구조 이해
> * RDB 관점에서 Vector DB 흐름 체득

---

## 1. 사전 조건

* Docker로 Chroma DB 실행 중
* 포트: `8000`
* Base URL:

```
http://localhost:8000
```

---

## 2. Chroma 핵심 개념 (아주 짧게)

| 개념         | 설명          | RDB 대응           |
| ---------- | ----------- | ---------------- |
| Collection | 벡터를 저장하는 단위 | Table            |
| Document   | 원문 텍스트      | Row              |
| Embedding  | 텍스트의 벡터 표현  | Index            |
| Query      | 벡터 유사도 검색   | WHERE + ORDER BY |

---

## 3. Postman 공통 설정

### Headers (모든 요청 공통)

```
Content-Type: application/json
```

---

## 4. Collection 목록 조회

### Request

* **Method**: GET
* **URL**:

```
/api/v1/collections
```

### Full URL

```
http://localhost:8000/api/v1/collections
```

### 기대 결과

* 현재 생성된 collection 리스트 반환

---

## 5. Collection 생성

### Request

* **Method**: POST
* **URL**:

```
/api/v1/collections
```

### Body (raw / JSON)

```json
{
  "name": "my_collection"
}
```

### 의미

* `my_collection`이라는 벡터 테이블 생성
* 이미 존재하면 에러

---

## 6. Collection 조회 (단건)

### Request

* **Method**: GET
* **URL**:

```
/api/v1/collections/my_collection
```

---

## 7. 문서(Document) 추가

### Request

* **Method**: POST
* **URL**:

```
/api/v1/collections/my_collection/add
```

### Body (raw / JSON)

```json
{
  "ids": ["doc-1", "doc-2"],
  "documents": [
    "Chroma는 벡터 데이터베이스입니다.",
    "Postman으로 Chroma API를 테스트하고 있습니다."
  ],
  "metadatas": [
    {"source": "intro"},
    {"source": "postman"}
  ]
}
```

### 내부 동작 (중요)

1. 문서를 embedding 벡터로 변환
2. 벡터 + 메타데이터 저장
3. 별도 embedding 요청 필요 없음

---

## 8. 저장된 문서 조회 (get)

### Request

* **Method**: POST
* **URL**:

```
/api/v1/collections/my_collection/get
```

### Body

```json
{
  "include": ["documents", "metadatas"]
}
```

---

## 9. 벡터 유사도 검색 (핵심)

### Request

* **Method**: POST
* **URL**:

```
/api/v1/collections/my_collection/query
```

### Body

```json
{
  "query_texts": ["벡터 데이터베이스란"],
  "n_results": 2
}
```

### 의미

* `query_texts`: 검색 질문
* `n_results`: 가장 유사한 문서 개수

### 결과 예시 구조

```json
{
  "ids": [["doc-1"]],
  "documents": [["Chroma는 벡터 데이터베이스입니다."]],
  "metadatas": [[{"source": "intro"}]],
  "distances": [[0.12]]
}
```

---

## 10. Collection 삭제 (주의)

### Request

* **Method**: DELETE
* **URL**:

```
/api/v1/collections/my_collection
```

---

## 11. 이 단계에서 반드시 이해해야 할 포인트

* Chroma는 **SQL이 없다**
* INSERT 대신 `add`
* SELECT 대신 `query`
* WHERE 조건은 **의미 기반 유사도**

---

## 12. 다음 단계 (아직 하지 말아도 됨)

* Spring Boot에서 Chroma REST 호출
* Collection 상태 대시보드 UI
* RAG 파이프라인 구성

---

## 요약

이 README 수준까지 이해하면:

* Vector DB를 DB처럼 다룰 수 있음
* RAG에서 Retrieval 단계 감 잡힘
* Spring 연동 준비 완료 상태

---
