# 🔥 Firebase Firestore 설정 가이드

## 1단계: Firebase 프로젝트 생성

1. [Firebase Console](https://console.firebase.google.com/)에 접속
2. "프로젝트 추가" 클릭
3. 프로젝트 이름 입력 (예: "hoon-studio-portfolio")
4. Google 애널리틱스는 선택사항 (필요시 체크)
5. "프로젝트 만들기" 클릭

## 2단계: Firestore 데이터베이스 설정

1. 왼쪽 메뉴에서 "Firestore Database" 클릭
2. "데이터베이스 만들기" 클릭
3. **테스트 모드로 시작** 선택 (나중에 보안 규칙 설정 가능)
4. 위치 선택 (한국이면 `asia-northeast3 (Seoul)` 권장)
5. "완료" 클릭

## 3단계: 웹 앱 설정

1. 프로젝트 설정 (톱니바퀴 아이콘) 클릭
2. "일반" 탭에서 "앱" 섹션 찾기
3. 웹 아이콘 (`</>`) 클릭
4. 앱 닉네임 입력 (예: "admin-panel")
5. **"Firebase Hosting도 설정"은 체크하지 않음**
6. "앱 등록" 클릭

## 4단계: 설정값 복사

```javascript
// 아래와 같은 설정값이 나타납니다:
const firebaseConfig = {
  apiKey: "AIzaSyB...",
  authDomain: "your-project.firebaseapp.com",
  projectId: "your-project-id",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef123456"
};
```

## 5단계: admin.html에 설정값 입력

`admin.html` 파일의 다음 부분을 찾아서 수정하세요:

```javascript
// Firebase 설정 - 여기에 본인의 Firebase 설정값을 입력하세요
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",                    // ← 여기에 복사한 값 입력
    authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
    projectId: "YOUR_PROJECT_ID",              // ← 여기에 복사한 값 입력
    storageBucket: "YOUR_PROJECT_ID.appspot.com",
    messagingSenderId: "YOUR_SENDER_ID",       // ← 여기에 복사한 값 입력
    appId: "YOUR_APP_ID"                       // ← 여기에 복사한 값 입력
};
```

### 예시:
```javascript
const firebaseConfig = {
    apiKey: "AIzaSyBdVdZc3F2_R1a2b3c4d5e6f7g8h9i0j1k",
    authDomain: "hoon-studio-portfolio.firebaseapp.com",
    projectId: "hoon-studio-portfolio",
    storageBucket: "hoon-studio-portfolio.appspot.com",
    messagingSenderId: "123456789012",
    appId: "1:123456789012:web:a1b2c3d4e5f6g7h8i9j0k1l2"
};
```

## 6단계: 보안 규칙 설정 (선택사항)

개발 후 실제 운영시에는 보안 규칙을 설정하는 것이 좋습니다:

1. Firestore Database > 규칙 탭 클릭
2. 다음 규칙 적용:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // projects 컬렉션: 읽기는 모두, 쓰기는 인증된 사용자만
    match /projects/{document} {
      allow read: if true;
      allow write: if request.auth != null;
    }
  }
}
```

## 7단계: 테스트

1. `admin.html` 파일을 브라우저에서 열기
2. 관리자 로그인 (admin / 1234)
3. 상단에 "🔥 Firebase 연결됨" 메시지 확인
4. 프로젝트 추가 테스트

## 문제 해결

### ❌ "Firebase 연결 실패" 에러
- 설정값이 정확한지 확인
- 프로젝트 ID가 올바른지 확인
- 브라우저 콘솔에서 에러 메시지 확인

### ❌ "Permission denied" 에러
- Firestore 보안 규칙이 너무 제한적인지 확인
- 테스트 모드로 변경 후 재시도

### ❌ 데이터가 저장되지 않는 경우
- 브라우저 콘솔에서 오류 확인
- 이미지 파일이 너무 큰지 확인 (Firestore 문서 크기 제한: 1MB)

## 💡 팁

1. **이미지 압축**: 자동으로 이미지를 압축하므로 큰 이미지도 업로드 가능
2. **무료 할당량**: Firebase 무료 플랜으로도 충분히 사용 가능
   - 문서 읽기: 50,000/일
   - 문서 쓰기: 20,000/일
   - 저장용량: 1GB
3. **백업**: Firebase Console에서 언제든 데이터 확인 및 백업 가능

이제 모든 컴퓨터에서 동일한 프로젝트 데이터를 볼 수 있습니다! 🎉 