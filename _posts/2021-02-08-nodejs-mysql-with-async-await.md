---
title: "Async/Await with MySQL"
excerpt: "Node.js와 MySQL연동 그리고 Async/Await"
toc: true
toc_sticky: true
header:
teaser: /assets/images/bio-photo-keyboard-teaser.jpg
categories:
  - Async/Await
tags:
  - Javascript
  - NodeJS
  - MySQL
  - Promise
  - Async/Await
  - Express
last_modified_at: 2021-02-07
---

# INTRO

저는 Twitter를 통해 알게된 지인분들과 함께 JS 스터디를 진행하고 있어요. 프론트엔드 개발자를 꿈꾸시는 분들이기에 백엔드 개발자인 제가 가르쳐드릴 수 있는게 무엇이 있을까 고민을 많이했어요. DBA가 되겠다며 오로지 DB만 공부했던 학부생 시절이 떠오르며, 한 가지만 공부한다고 개발을 잘 할 수 없다는걸 깨닫기 까지 시간이 오래걸렸었던 기억이 나서 제 과거 이야기를 들려 드린 후 Nodejs + Express + MySQL를 사용한 REST API를 개발하는 미니 프로젝트를 진행하고 있네요.

처음엔 index.js 파일 안에 모든 로직을 작성했지만 Route-Controller-Service 패턴을 적용해 목적에 맞게 코드를 분리하고 이제는 MySQL을 연동하여 Async/Await도 적용하여 제법 그럴싸한 REST API 서버의 형태를 가지게 됐네요.

주 마다 1,2회씩 스터디를 진행하기 위해 준비하면서 자료를 구하는것이 제일 어려웠던게 MySQL과 연동이었어요. 커넥션 객체를 생성하여 쿼리를 추출해 결과를 얻기까지의 과정을 정리해놓으신 분들은 많았지만 비동기로 처리되는 connection.query() 로직을 어떻게 다뤄야하는지에 대한 글은 몇 없더라구요. mysql2를 사용해 해결한 포스팅은 참 많이 보였는데, 저는 불가피한 경우가 아니라면 기본 모듈로 해결하는걸 좋아해서 mysql모듈만 가지고 놀아볼게요.

# Nodejs 와 MySQL

이 둘의 연동은 어려울게 없죠. mysql 모듈을 설치하고, 커넥션을 생성할 때 옵션값들을 넣어준 뒤 커넥션을 생성해 query를 추출하면 됩니다. Dotenv와 Pooling 기법을 사용한 샘플 예제를 보도록 하죠.

```javascript
const mysql = require("mysql");
require("dotenv").config();

// 커넥션 풀링
const connPool = mysql.createPool({
  host: process.env.DB_HOST,
  port: process.env.DB_PORT,
  user: process.env.DB_USER,
  password: process.env.DB_PW,
  database: process.env.DB_NAME,
  multipleStatements: process.env.DB_MULTIPLE_STATEMENTS,
  waitForConnections: process.env.WAIT_FOR_CONNECTIONS,
  connectionLimit: process.env.DB_CONN_LIMIT,
});

const query = "this is query !";

// 커넥션을 하나 가져와 쿼리를 추출합니다.
connPool.getConnection(function (err, conn) {
  if (err) {
    console.log(err);
    conn.release();
  } else {
    // 정상적으로 연결됐을 경우 query 추출
    conn.query(query, params, function (err, results, fields) {
      if (err) {
        console.log(err);
        conn.release();
      } else {
        console.log(results);
        console.log(fields);
        conn.release();
      }
    });
  }
});
```

서버는 클라이언트의 요청에 아래와 같은 흐름으로 응답해야합니다.

```
Request 발생 -> 데이터 추출 -> Resonse 에 추출된 데이터를 함께 보냄.
```

하지만 위 예제 코드에서 보이듯, 커넥션 객체에서 getConnection 후 query를 추출하려면 callback을 두 번 사용해야해요.
콜백은 비동기 처리 되기 때문에 내 뜻대로 제어할 수 없기 때문에 만약 저대로라면 아래와 같이 처리될거예요.

```
Request 발생->Response 먼저 가, 데이터는 조금 있다 추출할게->데이터 추출->클라이언트:???
```

혹은, 쿼리를 추출할 때 응답객체를 파라미터로 넣어서 콜백안에서 직접 응답을 보내도록 하는 방법도 있지만 Route-Conctoller-Service 패턴에서 Request,Response 객체는 Controller에서 관리를 하기에 비지니스로직이 쓰이는 service나 dao에 Response객체를 직접 넘겨주는건..음.. 추천하지 않아요. 그렇다면 어떤 방법으로 해결해야할까요?

# With Promise and Async/Await

Javascript는 Promise라는것이 있죠. 간단해요. 우선 몽땅 Promise 로 감싸보도록 하죠.

```javascript
const executeQuery = function (query, params) {
  return new Promise(function (resolve, reject) {
    connPool.getConnection(function (err, conn) {
      if (err) {
        console.log(err);
        conn.release();
        reject(err);
      } else {
        // 정상적으로 연결됐을 경우 query 추출
        conn.query(query, params, function (err, results, fields) {
          if (err) {
            console.log(err);
            conn.release();
            reject(err);
          } else {
            console.log(results);
            console.log(fields);
            conn.release();
            resolve(results);
          }
        });
      }
    });
  });
};
```

위 예제는 쿼리 추출로직을 Promise를 반환하도록 감싸 하나의 함수로 만든 예제입니다. 정상적으로 커넥션 객체를 가져와 쿼리를 추출하면 resolve에 results를 담아 반환하도록 만든 함수이죠.
이렇게 작성된 파일을 mysql.js으로 정하고 userDao.js에서 사용하고자 한다면 아래 같이 코딩할 수 있어요.

```javascript
// userDao.js
const db = require("utils/mysql.js");

const getAUser = async function (id) {
  const user = await db.executeQuery(`SELECT * FROM USER_TB WHERE ID = ?`, [
    id,
  ]);
  return user;
};
```

executeQuery함수는 Promise를 반환하기 때문에 async/await 을 사용해 깔끔하게 userDao.js 에서 쿼리 추출 결과를 받아 service에 반환할 수 있게 됐습니다!
