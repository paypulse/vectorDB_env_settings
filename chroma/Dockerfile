# Python 이 포함된 베이스 이미지 선택 
FROM python:3.11-slim 

WORKDIR /app

#Chroma db 설치 
RUN pip install --no-cache-dir chromadb==0.5.4 uvicorn fastapi 

#Port 열기
EXPOSE 8000

# chromadb 서버 실행 
CMD ["python", "-m", "chromadb.server.fastapi"]