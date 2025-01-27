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
#### 4. 무제한 API 요청 (bucket4j 라이브러리 설치 필수)
```
bucket4j.limits[0].name=rate_limit
bucket4j.limits[0].requests=10  # 최대 10회 요청 허용
bucket4j.limits[0].interval=60  # 60초 내에 10회 요청 허용
```
#### 5. 무제한 캐싱  
```
spring.cache.type=ehcache # 캐시 타입을 Ehcache로 설정
spring.cache.ehcache.config=classpath:ehcache.xml # 캐시 파일 위치 및 설정 파일 경로 (Ehcache 설정 파일)
spring.cache.ehcache.time-to-live=3600s  # 1시간 / 이 설정을 위에 설정 파일로 관리 가능  
spring.cache.ehcache.max-entries=1000    # 캐시 최대 항목 수 / 이 설정을 위에 설정 파일로 관리 가능  
```
#### 6. 무제한 입력 검증 미비(JS 작성 / vo,dto,dao 등에 작성)  
```
<script>
    document.getElementById("loginForm").addEventListener("submit", function(event) {
        event.preventDefault(); // 기본 제출 동작 방지

        // 입력 필드와 오류 메시지 요소
        const username = document.getElementById("username");
        const password = document.getElementById("password");
        const usernameError = document.getElementById("usernameError");
        const passwordError = document.getElementById("passwordError");

        // 초기화
        usernameError.textContent = "";
        passwordError.textContent = "";

        let isValid = true;

        // 아이디 검증
        if (username.value.trim() === "") {
            usernameError.textContent = "아이디를 입력해주세요.";
            isValid = false;
        } else if (username.value.length < 4 || username.value.length > 12) {
            usernameError.textContent = "아이디는 4~12자 사이여야 합니다.";
            isValid = false;
        }

        // 비밀번호 검증
        if (password.value.trim() === "") {
            passwordError.textContent = "비밀번호를 입력해주세요.";
            isValid = false;
        } else if (!/^(?=.*[A-Za-z])(?=.*\d)(?=.*[@$!%*#?&])[A-Za-z\d@$!%*#?&]{8,}$/.test(password.value)) {
            passwordError.textContent = "비밀번호는 8자 이상, 문자, 숫자, 특수문자를 포함해야 합니다.";
            isValid = false;
        }

        // 최종 확인
        if (isValid) {
            alert("로그인 성공!");
            // 실제로 서버로 데이터를 보낼 경우 여기에서 처리
            // this.submit();
        }
    });
</script>
```
```
@NotEmpty(message = "이름은 필수 항목입니다.")
@Size(min = 3, max = 50, message = "이름은 3자 이상 50자 이하로 입력하세요.")
private String name;
```

