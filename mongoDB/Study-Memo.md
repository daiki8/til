# mongoDB Study Memo

mogo playground
https://mongoplayground.net/


sample data(bson)
```
[
  {
    "_id": ObjectId("61dc34aa728087e3b09d7e22"),
    "title": "Task 1",
    "description": "This is the first task",
    "status": "completed"
  },
  {
    "_id": ObjectId("61dc34aa728087e3b09d7e23"),
    "title": "Task 2",
    "description": "This is the second task",
    "status": "in progress"
  },
  {
    "_id": ObjectId("61dc34aa728087e3b09d7e24"),
    "title": "Task 3",
    "description": "This is the third task",
    "status": "not started"
  }
]
```

## クエリ例

すべてのタスクを取得する
```
db.collection.find()
```

result
```
[
  {
    "_id": ObjectId("61dc34aa728087e3b09d7e22"),
    "description": "This is the first task",
    "status": "completed",
    "title": "Task 1"
  },
  {
    "_id": ObjectId("61dc34aa728087e3b09d7e23"),
    "description": "This is the second task",
    "status": "in progress",
    "title": "Task 2"
  },
  {
    "_id": ObjectId("61dc34aa728087e3b09d7e24"),
    "description": "This is the third task",
    "status": "not started",
    "title": "Task 3"
  }
]
```



特定のタスクをタイトルで検索するクエリ
```
db.collection.find({
  title: "Task 1"
})
```

result
```
[
  {
    "_id": ObjectId("61dc34aa728087e3b09d7e22"),
    "description": "This is the first task",
    "status": "completed",
    "title": "Task 1"
  }
]
```

特定のステータスを持つタスクを取得するクエリ
```
db.collection.find({
  status: "completed"
})
```

result
```
[
  {
    "_id": ObjectId("61dc34aa728087e3b09d7e22"),
    "description": "This is the first task",
    "status": "completed",
    "title": "Task 1"
  }
]
```


タスクのタイトルと説明を取得するクエリ
```
db.collection.find({},
{
  title: 1,
  description: 1
})
```

result
```
[
  {
    "_id": ObjectId("61dc34aa728087e3b09d7e22"),
    "description": "This is the first task",
    "title": "Task 1"
  },
  {
    "_id": ObjectId("61dc34aa728087e3b09d7e23"),
    "description": "This is the second task",
    "title": "Task 2"
  },
  {
    "_id": ObjectId("61dc34aa728087e3b09d7e24"),
    "description": "This is the third task",
    "title": "Task 3"
  }
]
```



ステータスごとにタスクをグループ化するクエリ
```
db.collection.aggregate([
  {
    $group: {
      _id: "$status",
      tasks: {
        $push: "$title"
      }
    }
  }
])
```

result
```
[
  {
    "_id": "in progress",
    "tasks": [
      "Task 2"
    ]
  },
  {
    "_id": "not started",
    "tasks": [
      "Task 3"
    ]
  },
  {
    "_id": "completed",
    "tasks": [
      "Task 1"
    ]
  }
]
```


タスクの状態ごとにカウントを集計
```
db.collection.aggregate([
  {
    $group: {
      _id: "$status",
      count: {
        $sum: 1
      }
    }
  },
  {
    $sort: {
      count: -1
    }
  }
```
