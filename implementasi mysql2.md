Halo pada tulisan ini saya akan implementasi auth menggunakan bcrypt

1. Menambahkan mysql2 pada project kita.<br>
   ```npm
   npm i mysql2
   ```
2. mengimport mysql2<br>
   ```js
   var mysql = require("mysql2");
   ```
3. Membuat config file untuk digunakan di dalam mysql2
   <br><br>
   **config.js**
   <br><br>
   ```js
    await mysql.createConnection({
        host: 'localhost',
        user: 'root',
        password: 'password',
        database: 'database'
    });
   ```
4. Example of mysqly2 promise
   <br><br>
   **app.js**
   <br><br>
   ```js
    const mysql = require('mysql2/promise');
    const connection = await mysql.createConnection({
        host: 'localhost',
        user: 'root',
        password: 'password',
        database: 'database'
    });
    const [rows, fields] = await connection.execute('SELECT * FROM `table`');
    console.log(rows); // results in an array of rows
    console.log(fields); // results in an array of columns
```