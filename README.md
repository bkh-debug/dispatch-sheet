# 출고지시서 관리 웹앱

> ✅ **Firebase 설정 완료됨**: `dispatch-sheet-bd0e2` 프로젝트가 이미 생성되어 있고, Firestore(서울 리전) 데이터베이스와 보안 규칙, 웹 앱 등록, `index.html`의 firebaseConfig까지 전부 연결된 상태입니다. 아래 1~2단계는 이미 완료되어 있으니 **3단계(GitHub에 올리기)**부터 진행하시면 됩니다.


엑셀 "신규출고지시서.xlsx"를 대체하는 웹 앱입니다.
- 데이터 저장: **Firebase Firestore** (실시간 데이터베이스)
- 코드 보관 및 배포: **GitHub** + **GitHub Pages** (무료 웹 호스팅)
- 별도 서버, 별도 설치 프로그램 없이 브라우저만으로 동작합니다.

아래 순서를 그대로 따라 하시면 됩니다. 전체 소요 시간은 20~30분 정도예요.

---

## 0. 준비물

- Google 계정 (Firebase 사용)
- GitHub 계정 (없다면 https://github.com/signup 에서 무료로 생성)

---

## 1단계: Firebase 프로젝트 만들기

1. https://console.firebase.google.com 접속 → **프로젝트 추가**
2. 프로젝트 이름 입력 (예: `dispatch-sheet`) → 계속 → Google 애널리틱스는 꺼도 됩니다 → **프로젝트 만들기**
3. 왼쪽 메뉴 **빌드 > Firestore Database** 클릭 → **데이터베이스 만들기**
4. 위치는 `asia-northeast3 (Seoul)` 선택 → **테스트 모드로 시작** 선택 → 사용 설정
5. 왼쪽 메뉴 상단 **프로젝트 개요 옆 톱니바퀴 > 프로젝트 설정** 클릭
6. 아래로 스크롤 → **내 앱** 섹션에서 `</>` (웹) 아이콘 클릭
7. 앱 닉네임 입력 (예: `dispatch-web`) → **앱 등록** (Firebase Hosting 체크는 안 해도 됩니다)
8. 화면에 나오는 `firebaseConfig` 값을 복사해두세요. 이렇게 생겼습니다:

```js
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "dispatch-sheet.firebaseapp.com",
  projectId: "dispatch-sheet",
  storageBucket: "dispatch-sheet.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef"
};
```

### Firestore 보안 규칙 적용

1. Firestore Database 화면 → **규칙** 탭
2. 이 폴더의 `firestore.rules` 파일 내용을 복사해서 붙여넣고 **게시** 클릭
   (테스트/사내 용도용 임시 규칙입니다. `firestore.rules` 파일 상단 주석 참고)

---

## 2단계: 앱에 Firebase 연결하기

1. 이 폴더의 **`index.html`** 파일을 메모장(또는 VS Code)으로 엽니다
2. 파일 안에서 아래 부분을 찾습니다 (위에서 40번째 줄 근처, `<script>` 태그 안):

```js
const firebaseConfig = {
  apiKey: "여기에_API_KEY_붙여넣기",
  authDomain: "여기에_PROJECT_ID.firebaseapp.com",
  ...
};
```

3. 1단계 8번에서 복사한 본인의 `firebaseConfig` 값으로 **통째로 교체**합니다
4. 저장합니다

이제 `index.html`을 더블클릭해서 브라우저로 열어보면 상단에 "Firestore 연결됨"이 뜨는지 확인하세요.
연결되면 화면에 뜨는 **"제품 마스터 가져오기"** 버튼을 한 번 눌러주세요. (최초 1회만, 엑셀에 있던 제품코드 251건이 자동으로 들어갑니다)

---

## 3단계: GitHub에 올리고 무료로 배포하기

컴퓨터마다 매번 파일을 여는 대신, 인터넷 주소로 접속해서 여러 명이 동시에 쓰려면 GitHub Pages를 씁니다.

1. https://github.com 로그인 → 오른쪽 위 **+** → **New repository**
2. Repository name 입력 (예: `dispatch-sheet`) → Public 선택 → **Create repository**
3. 생성된 저장소 페이지에서 **Add file > Upload files** 클릭
4. 이 폴더 안의 `index.html` 파일을 끌어다 놓기 (firestore.rules, README.md는 안 올려도 되지만 같이 올려도 무방합니다)
5. 하단 **Commit changes** 클릭
6. 저장소 상단 **Settings** 탭 → 왼쪽 메뉴 **Pages**
7. **Source**를 `Deploy from a branch`로, Branch는 `main` / `/ (root)` 선택 → **Save**
8. 1~2분 후 같은 화면에 뜨는 주소로 접속하면 됩니다
   (예: `https://아이디.github.io/dispatch-sheet/`)

이후 파일을 수정하고 싶으면 저장소에서 `index.html`을 다시 업로드(덮어쓰기)하면 자동 반영됩니다.

---

## 4단계: 사용 방법

- **출고 등록**: 왼쪽 패널에서 날짜, 제품코드(입력하면 제품명이 자동으로 뜹니다), 출고방식(EP1/메일), 1~4호차 수량을 입력하고 **출고 등록** 클릭
- **출고 목록**: 오른쪽 패널에서 날짜를 바꾸면 그날 등록된 내역이 실시간으로 보입니다 (다른 사람이 등록해도 화면이 자동으로 갱신돼요)
- **삭제**: 각 행 오른쪽 삭제 버튼
- **CSV 내보내기**: 현재 보고 있는 날짜의 내역을 엑셀에서 열 수 있는 CSV 파일로 다운로드

---

## 데이터 구조 (참고용)

Firestore에는 두 개의 컬렉션(테이블)이 생깁니다.

**`products`** — 제품코드 마스터 (엑셀 N/O열에서 가져옴)
```
{ code: "3300", name: "4315(검정)" }
```

**`orders`** — 실제 출고 등록 내역 (엑셀 B~M열에 해당)
```
{
  date: "2026-07-01",
  productCode: "3300",
  productName: "4315(검정)",
  shipType: "EP1",
  v1: 72, v2: 0, v3: 72, v4: 0,
  qty: 144,
  ep1Qty: 144, mailQty: 0,
  note: "A동 2층",
  createdAt: <서버 시간>
}
```

---

## 나중에 더 안전하게 만들고 싶다면

지금 규칙은 링크만 알면 누구나 데이터를 읽고 쓸 수 있는 "테스트 모드"입니다. 사내 링크로만 공유한다면 큰 문제는 없지만, 나중에 직원별 로그인이 필요해지면:

1. Firebase 콘솔 > **Authentication** > 로그인 방법에서 이메일/비밀번호 또는 Google 로그인 사용 설정
2. `firestore.rules`의 `allow read, write: if true;` 를 `allow read, write: if request.auth != null;` 로 변경
3. `index.html`에 로그인 화면 추가 (필요하시면 다음에 이어서 만들어 드릴게요)

---

## 문제 해결

- **"Firebase 설정 필요"라고 뜰 때** → 2단계에서 `firebaseConfig`를 제대로 붙여넣었는지 확인
- **저장은 되는데 목록에 안 보일 때** → 조회 날짜가 등록한 날짜와 같은지 확인
- **"Missing or insufficient permissions" 오류** → Firestore 규칙을 게시했는지, 오탈자가 없는지 확인
