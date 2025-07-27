# 🚨 Firestore 문제 해결 가이드

## 문제: 프로젝트 등록 시 계속 로딩이 뜨는 경우

### 🔍 1단계: Firestore 데이터베이스 생성 확인

1. [Firebase Console](https://console.firebase.google.com/) 접속
2. "the-hoon-studio" 프로젝트 선택
3. 왼쪽 메뉴에서 **"Firestore Database"** 클릭

### 📋 경우별 해결방법

#### 경우 1: "데이터베이스 만들기" 버튼이 보이는 경우
→ **Firestore가 아직 생성되지 않음**

**해결 단계:**
1. "데이터베이스 만들기" 클릭
2. **"테스트 모드로 시작"** 선택
3. 위치: `asia-northeast3 (Seoul)` 선택
4. "완료" 클릭

#### 경우 2: Realtime Database만 있는 경우
→ **잘못된 데이터베이스 타입**

**해결 단계:**
1. 왼쪽 메뉴에서 "Firestore Database" 찾기
2. 위의 "경우 1" 단계 따라하기

#### 경우 3: Firestore는 있지만 보안 규칙 문제
→ **접근 권한 오류**

**해결 단계:**
1. Firestore Database → "규칙" 탭 클릭
2. 다음 규칙으로 변경:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;
    }
  }
}
```

3. "게시" 클릭

### 🔧 2단계: 브라우저 콘솔 확인

**F12 → Console 탭에서 확인할 에러들:**

#### ❌ "Missing or insufficient permissions" 에러
→ 보안 규칙 문제 (위의 경우 3 해결)

#### ❌ "FirebaseError: Firebase: No Firebase App" 에러  
→ Firebase 설정 오류

#### ❌ "The caller does not have permission" 에러
→ Firestore 보안 규칙 문제

### 🎯 3단계: 설정 파일 수정

현재 설정에 불필요한 항목들이 있습니다:

**수정 전:**
```javascript
const firebaseConfig = {
  apiKey: "AIzaSyDUJrHXzQfZ2-FwaZcuMcLu-71gRTVO2b4",
  authDomain: "the-hoon-studio.firebaseapp.com",
  databaseURL: "https://the-hoon-studio-default-rtdb.europe-west1.firebasedatabase.app", // ← 삭제
  projectId: "the-hoon-studio",
  storageBucket: "the-hoon-studio.firebasestorage.app",
  messagingSenderId: "857575526697",
  appId: "1:857575526697:web:43d66881a72f96e5a6e738",
  measurementId: "G-RH3T4RFD70" // ← 삭제 (선택사항)
};
```

**수정 후:**
```javascript
const firebaseConfig = {
  apiKey: "AIzaSyDUJrHXzQfZ2-FwaZcuMcLu-71gRTVO2b4",
  authDomain: "the-hoon-studio.firebaseapp.com",
  projectId: "the-hoon-studio",
  storageBucket: "the-hoon-studio.firebasestorage.app",
  messagingSenderId: "857575526697",
  appId: "1:857575526697:web:43d66881a72f96e5a6e738"
};
```

### 🧪 4단계: 테스트

1. 브라우저 새로고침 (F5)
2. 관리자 로그인
3. 상단 메시지 확인:
   - ✅ "🔥 Firebase 연결됨" → 정상
   - ❌ "❌ Firebase 연결 실패" → 1-3단계 재확인

### 🆘 여전히 문제가 있다면

**브라우저 콘솔의 정확한 에러 메시지를 확인하고 다음 정보를 제공해주세요:**

1. 콘솔에 나타나는 빨간색 에러 메시지
2. Firebase Console에서 Firestore Database가 정상적으로 보이는지
3. "🔥 Firebase 연결됨" 메시지가 보이는지

### 💡 추가 팁

- **Firestore vs Realtime Database**: 우리는 Firestore를 사용합니다
- **테스트 모드**: 개발 중에는 테스트 모드로 사용하세요
- **보안 규칙**: 나중에 실제 운영시에만 엄격하게 설정하세요 