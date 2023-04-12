# Pinecone Memo

## Quickstart

https://docs.pinecone.io/docs/quickstart

python3インストール
```
python -V
```

python実行
```
python
```

pineconeインポート
pinecone初期化
```
import pinecone
pinecone.init(api_key="YOUR_API_KEY", environment="YOUR_ENVIRONMENT")
```

インデックス作成
8次元ベクトルのユークリッド距離のなんちゃら
```
pinecone.create_index("quickstart", dimension=8, metric="euclidean", pod_type="p1")
```

インデックスのリスト取得
```
pinecone.list_indexes()
# Returns:
# ['quickstart']
```

利用するindex
```
index = pinecone.Index("quickstart")
```

サンプルデータを追加
```
# Upsert sample data (5 8-dimensional vectors)
index.upsert([
    ("A", [0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1]),
    ("B", [0.2, 0.2, 0.2, 0.2, 0.2, 0.2, 0.2, 0.2]),
    ("C", [0.3, 0.3, 0.3, 0.3, 0.3, 0.3, 0.3, 0.3]),
    ("D", [0.4, 0.4, 0.4, 0.4, 0.4, 0.4, 0.4, 0.4]),
    ("E", [0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5])
])
```

類似のベクトルを取得
```
index.describe_index_stats()
# Returns:
# {'dimension': 8, 'index_fullness': 0.0, 'namespaces': {'': {'vector_count': 5}}}
```

インデックスの削除
```
index.query(
  vector=[0.3, 0.3, 0.3, 0.3, 0.3, 0.3, 0.3, 0.3],
  top_k=3,
  include_values=True
)
# Returns:
# {'matches': [{'id': 'C',
#               'score': 0.0,
#               'values': [0.3, 0.3, 0.3, 0.3, 0.3, 0.3, 0.3, 0.3]},
#              {'id': 'D',
#               'score': 0.0799999237,
#               'values': [0.4, 0.4, 0.4, 0.4, 0.4, 0.4, 0.4, 0.4]},
#              {'id': 'B',
#               'score': 0.0800000429,
#               'values': [0.2, 0.2, 0.2, 0.2, 0.2, 0.2, 0.2, 0.2]}],
#  'namespace': ''}
```

```
pinecone.delete_index("quickstart")
```

## 関連

https://github.com/Torantulino/Auto-GPT

