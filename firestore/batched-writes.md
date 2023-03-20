# batched-writes

一度に最大500件

よく見るやつ
```
// Firebase Admin SDKを使用してFirestoreデータベースのインスタンスを作成。Firestoreデータベースへのアクセスが可能になる
const db = admin.firestore();
// Firestoreデータベースのバッチ処理インスタンスを作成。このインスタンスを使用して、一連の書き込み操作をグループ化し、一度にコミットすることができる
const batch = db.batch();
// Firestoreデータベース内の指定されたコレクション（collectionName）に新しいドキュメントを作成。docメソッドを引数なしで呼び出すとドキュメントIDは自動的に生成。https://firebase.google.com/docs/firestore/manage-data/add-data?hl=ja
const docRef = db.collection(collectionName).doc();
// docRefで指定されたドキュメントに対して、dataオブジェクトを使用してドキュメントの内容を設定（新規作成または上書き）する操作をバッチに追加
batch.set(docRef, data);
// バッチに追加されたすべての操作を実行し、コミット。この操作は非同期であり、Promiseを返す。コミットが成功すると、バッチ内のすべての操作が原子的に実行。失敗した場合、バッチ内の操作はすべてロールバック。
batch.commit()
```
