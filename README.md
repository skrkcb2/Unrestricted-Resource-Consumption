# Unrestricted-Resource-Consumption
### 특징: 자원의 고정하지 않았을때 발생하는 취약점이다
### 취약점, 보안 방안, 공격 예시(기준 스프링 부트)
#### 1. 파일 업로드 크기 제한 미설정  
```
spring.servlet.multipart.max-file-size=10MB # 파일 업로드 크기 10MB
```
#### 2. 무제한 요청 크기  
```
spring.servlet.multipart.max-request-size=10MB # request 크기 10MB
```
#### 3. 무제한 세션 저장소 크기
```
server.servlet.session.timeout=30m # 세션 타임아웃 30분
or redis(세션 및 토큰 저장소) 사용
```
#### 4. 무제한 API 요청
```
bucket4j.limits[0].name=rate_limit
bucket4j.limits[0].requests=10  # 최대 10회 요청 허용
bucket4j.limits[0].interval=60  # 60초 내에 10회 요청 허용
```
#### 5. 무제한 캐싱  
#### 6. 무제한 입력 검증 미비  
#### 7. 무제한 스크립트 실행  
#### 8. 무제한 템플릿 렌더링  

