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

## 강좌

- [4.2 Uploading](https://nomadcoders.co/nwitter/lectures/1927)
