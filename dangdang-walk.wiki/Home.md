# 🔒OAuth 사전 작업

## 🟨[Kakao Developers](https://developers.kakao.com/docs/latest/ko/kakaologin/prerequisite)

1. https://developers.kakao.com/console/app > 애플리케이션 추가하기
2. 애플리케이션 선택
3. 내 애플리케이션 > 앱 설정 > 요약 정보
   - 앱 키 > **_REST API 키_** (인가 코드 요청 시 필요)
4. 플랫폼 > 플랫폼 설정하기 > Web > **_사이트 도메인_**: http://localhost:3000
5. Redirect URI 등록하러 가기
   - 활성화 설정: ON
   - Redirect URI 등록 > **_Redirect URI_**: http://localhost:3000/callback
6. 내 애플리케이션 > 제품 설정 > 카카오 로그인 > 보안 > **_Client Secret_** > 코드 생성 (선택)
   - 활성화 상태: 사용함
7. 내 애플리케이션 > 제품 설정 > 카카오 로그인 > 동의항목

   - 닉네임
   - 프로필 사진
   - 카카오계정(이메일) 설정 👈 **비즈앱**으로 전환해야 동의항목 설정이 가능하다.

   추후 인가 코드를 받을 때 [scope](https://developers.kakao.com/docs/latest/ko/kakaologin/common#user-info-kakao-account)와 관련 있다.

## 🟩[Naver Developers](https://developers.naver.com/docs/common/openapiguide/appregister.md#%EC%95%A0%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98-%EB%93%B1%EB%A1%9D)

1. https://developers.naver.com/apps/#/wizard/register > 약관동의 > 계정 정보 등록 > 애플리케이션 등록
   - 사용 API: 네이버 로그인
     - 권한: 회원이름, 연락처 이메일 주소, 별명, 프로필 사진 ✔️
   - 로그인 오픈 API 서비스 환경: PC 웹
     - **_서비스 URL_**: http://localhost:3000
     - 네이버 로그인 **_Callback URL_**: http://localhost:3000/callback
2. 내 애플리케이션 > 개요
   - 애플리케이션 정보
     - **_Client ID_**
     - **_Client Secret_**
   - 네이버 로그인
     - 개발 상태 (개발 중)
       > 애플리케이션이 '개발 중' 상태이면 [멤버관리] 탭에서 등록한 아이디만 네이버 로그인을 이용할 수 있습니다. 개발이 완료되어 실 서비스에 적용하고자 하신다면 검수를 요청해 주세요. 검수가 승인되면 모든 아이디로 네이버 로그인을 이용할 수 있습니다.
3. 내 애플리케이션 > 멤버관리 (애플리케이션 개설자는 등록할 필요가 없다.)
   - 관리자 ID 등록
   - 테스터 ID 등록

## 🟦[Google Cloud Console](https://developers.google.com/identity/protocols/oauth2/web-server?hl=ko#creatingcred)

1. https://console.cloud.google.com > 프로젝트 선택 > 새 프로젝트
2. 왼쪽 탐색 메뉴 > API 및 서비스
   1. OAuth 동의 화면 > User Type: 외부 > 만들기
      - OAuth 동의 화면
        - 앱 도메인 > **_애플리케이션 홈페이지_**: http://localhost:3000
      - 범위 (추후 인가 코드 발급 시 scope에 사용됨)
        - 범위 추가 또는 삭제 > email, profile ✔️ > 업데이트
      - 테스트 사용자
        - 본인 메일 추가
   2. 사용자 인증 정보 > 사용자 인증 정보 만들기 > OAuth 클라이언트 ID
      - 애플리케이션 유형: 웹 애플리케이션
      - **_승인된 자바스크립트 원본_**: http://localhost:3000
      - **_승인된 리디렉션 URI_**: http://localhost:3000/callback
   3. 사용자 인증 정보 > OAuth 2.0 클라이언트 ID > 방금 만든 웹 클라이언트 클릭
      - **_클라이언트 ID_**
      - **_클라이언트 보안 비밀번호_**