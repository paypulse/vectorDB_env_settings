# 벡터 데이터베이스 설계 및 구현 

# VectorDb관련 docker file setting 

### Setting될 종류 

## 1. Chroma DB
    - 임베딩 저장 검색에 특화
    - Light weight 한 vector store 로컬 환경 실험에 적합 
    - 텍스트, 문서 같은걸 벡터로 저장하고 검색하는 Vector DB 이다.
    - ChromaDB 를 쓰면 자동으로 DuckDB가 깔리는데 벡터 DB의 엔진이자 내부 저장소야
    - dataGrip에서 duckDB로 접속 하면 될것 같다. 따로 GUI를 지원 하는것 같진 않다. 
    - postman에서 확인 한다. 

## 2. Neo4jDB
    - 관계형 Vector 확장을 위한 그래프 DB
    - 문서 간 개념 연결, 임베딩 노드 관리 
## 3. FAISS
    - 벡터 검색 엔진 
## 4. CHATSYSTEM
    - Chroma / FAISS / Neo4j와 연동 
    - 문서 검색 + 대화 시스템 

### 관련 링크 
https://blog.naver.com/ilifo_book 
