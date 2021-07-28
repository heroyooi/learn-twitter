# Learn Twitter

## 1.0 React + Firebase Setup

- 웹 앱에 Firebase 추가 > 앱 등록 > Firebase SDK 추가
- src/firebase.js 생성 후 아래 공식문서 방법대로 파이어베이스 연결
- [자바스크립트 프로젝트에 Firebase 추가](https://firebase.google.com/docs/web/setup?authuser=0#add-sdks-initialize)

```command
npm i firebase
```

## 1.2 Router Setup

```command
npm i react-router-dom
```

## 2.0 Using Firebase Auth

- [파이어베이스 - docs - firebaseauth](https://firebase.google.com/docs/reference/js/firebase.auth)

```json
{
  "compilerOptions": {
    "baseUrl": "src"
  },
  "include": ["src"]
}
```

- 전체 파일 import 루트를 src로 지정

## 2.3 Creating Account

- [파이어베이스 - docs - firebaseauth - createUserWithEmailAndPassword](https://firebase.google.com/docs/reference/js/firebase.auth.Auth#createuserwithemailandpassword)

## 2.4 Log In

- [파이어베이스 - docs - firebaseauth - onAuthStateChanged](https://firebase.google.com/docs/reference/js/firebase.auth.Auth#onauthstatechanged)

- onAuthStateChanged: 유저가 로그아웃할 때, 계정을 생성할 때, firebase가 초기화 될 때 실행

## 2.5 Social Login

- [파이어베이스 - docs - signInWithPopup](https://firebase.google.com/docs/reference/js/firebase.auth.Auth#signinwithpopup)

## 2.6 Log Out

- [React Router - Hooks - useHistory](https://reactrouter.com/web/api/Hooks/usehistory)

## 3.0 Form and Database Setup

- Firestore Database 사용
- fbase.js 파일에 데이터베이스 연결

```js
import "firebase/firestore";

export const dbService = firebase.firestore();
```

## 3.1 Nweeting

- [파이어베이스 - docs - Firestore](https://firebase.google.com/docs/reference/js/firebase.firestore.Firestore)

## 3.2 Getting the Nweets

- [파이어베이스 - docs - Firestore - CollectionReference](https://firebase.google.com/docs/reference/js/firebase.firestore.CollectionReference)
- [파이어베이스 - docs - Firestore - QuerySnapshot](https://firebase.google.com/docs/reference/js/firebase.firestore.QuerySnapshot)

```js
const Home = ({ userObj }) => {
  const [nweets, setNweets] = useState([]);
  const getNweets = async () => {
    const dbNweets = await dbService.collection("nweets").get();
    dbNweets.forEach((document) => {
      const nweetObject = {
        ...document.data(),
        id: document.id,
      };
      setNweets((prev) => [nweetObject, ...prev]);
    });
  };
  useEffect(() => {
    getNweets();
  }, []);
};
```

## 3.3 Realtime Nweets

- [파이어베이스 - docs - Firestore - CollectionReference - onSnapshot](https://firebase.google.com/docs/reference/js/firebase.firestore.CollectionReference#onsnapshot)

```js
const Home = ({ userObj }) => {
  const [nweet, setNweet] = useState("");
  const [nweets, setNweets] = useState([]);
  useEffect(() => {
    dbService.collection("nweets").onSnapshot((snapshot) => {
      const nweetArray = snapshot.docs.map((doc) => ({
        id: doc.id,
        ...doc.data(),
      }));
      setNweets(nweetArray);
    });
  }, []);
};
```

## 3.5 Delete and Update part Two

- [파이어베이스 - docs - Firestore - DocumentReference](https://firebase.google.com/docs/reference/js/firebase.firestore.DocumentReference)

## 4.0 Preview Images part One

- [MDN web docs - FileReader](https://developer.mozilla.org/ko/docs/Web/API/FileReader)

## 4.2 Uploading

```command
npm i uuid
```

- [파이어베이스 - docs - storage - Reference](https://firebase.google.com/docs/reference/js/firebase.storage.Reference)

## 4.3 File URL and Nweet

- [파이어베이스 - docs - storage - UploadTaskSnapshot](https://firebase.google.com/docs/reference/js/firebase.storage.UploadTaskSnapshot)

## 5.0 Get My Own Nweets

```js
import { dbService } from "fbase";

const getMyNweets = async () => {
  const nweets = await dbService
    .collection("nweets")
    .where("creatorId", "==", userObj.uid)
    .orderBy("createdAt")
    .get();
  console.log(nweets.docs.map((doc) => doc.data()));
};

useEffect(() => {
  getMyNweets();
}, []);
```

- 콘솔 에러에 링크를 클릭해 파이어베이스 색인을 추가한다.

## 5.1 Update Profile

- [파이어베이스 - docs - user - updateProfile](https://firebase.google.com/docs/reference/js/firebase.User#updateprofile)

## 6.1 Styles

```command
npm i @fortawesome/fontawesome-free @fortawesome/fontawesome-svg-core @fortawesome/free-brands-svg-icons @fortawesome/free-regular-svg-icons @fortawesome/free-solid-svg-icons @fortawesome/react-fontawesome
```

## 6.2 Deploying

```command
git remote -v
```

- 활성화된 원격저장소 이름 확인 (learn-twitter)
- package.json 파일에 homepage 작성: "https:<유저>.github.io/<저장소 이름>"

```json
{
  "homepage": "https://heroyooi.github.io/learn-twitter"
}
```

- create-react-app이 우리의 사이트를 빌드할 때 URL을 알아야한다.
- index.html의 %PUBLIC_URL% 이 부분이 homepage를 인식한다.

```command
npm i gh-pages
```

- package.json 파일에 deploy 작성

```json
{
  "scripts": {
    "predeploy": "npm run build",
    "deploy": "gh-pages -d build"
  }
}
```

```command
npm run deploy
```

## 6.3 Security on Firebase

- 파이어베이스 관리자에서 Authentication > Sign-in method > 승인된 도메인 > 도메인 추가
- heroyooi.github.io 입력

- [파이어베이스 - docs - 보안규칙 - 보안규칙 이해](https://firebase.google.com/docs/rules/rules-language?authuser=0)

## 6.4 API Key Security

- [구글 - 사용자 인증 정보](https://console.cloud.google.com/apis/credentials?folder=&organizationId=&project=nwitter-d8c07)

### API 키의 Browser key 클릭

- 애플리케이션 제한사항 - HTTP 리퍼러(웹사이트) 선택
- 웹사이트 제한사항 항목 추가 클릭(ADD AN ITEM)
- heroyooi.github.io/\* 추가
- localhost 추가
- nwitter-d8c07.firebaseapp.com/\* 추가 (파이어베이스 Authentication - 승인된 도메인에서 확인)
- 저장

### Oauth 2.0 클라이언트 ID 의 Web Client 클릭

- URI 추가 (ADD URI)

## 강좌

- [#6.0 Cleaning JS](https://nomadcoders.co/nwitter/lectures/1933)
- [강좌 소스](https://github.com/nomadcoders/nwitter)
- [배포 결과물](https://heroyooi.github.io/learn-twitter)
