# Belajar-NodeJS-ExpressJS

# Intro to Node.js

Contents:

- [Install Visual Studio Code](#install-visual-studio-code)
- [Node.js](#nodejs)
- [Membagi kode program ke beberapa file](#membagi-kode-program-ke-beberapa-file)
- [Events](#events)
- [Buffers](#buffers)
- [Streams](#streams)
- [File system](#file-system)
- [CLI](#cli)

## Install Visual Studio Code

[Download Visual Studio Code](https://code.visualstudio.com/download)

## Node.js

- JavaScript merupakan bahasa pemrograman populer yang digunakan untuk membuat tampilan web interaktif. Sebelum adanya Node.js, JavaScript hanya dapat berjalan pada web browser.
- Node.js merupakan sebuah runtime JavaScript yang berbasiskan **V8 JavaScript Engine**.
- Node.js dirilis tahun 2009 oleh Ryan Dahl. Saat ini Node.js sudah memiliki komunitas yang besar.
- Hadirnya Node.js, memungkinkan programmer untuk menggunakan JavaScript pada sisi backend (server).
- Dengan Node.js programmer dimungkinkan untuk membangun aplikasi fullstack (back-end sampai front-end) hanya dengan menggunakan JavaScript sebagai bahasa pemrogramannya.
- Node.js dapat bekerja secara asynchronous sehingga memungkinkan untuk membangun sebuah aplikasi yang bersifat scalable.


[Download Node.js](https://nodejs.org/en/download/) (install versi LTS)

### Hello world

**app.js**

```js
console.log('Hello world!')
```

### Hello world (web server)

**app.js**

```js
const http = require('http');

const server = http.createServer(function (req, res) {
  res.writeHead(200, {
    'Content-Type': 'text/html'
  });
  res.end('Hello world!')
});

server.listen(3000);
```

## Membagi kode program ke beberapa file

### Menggunakan `exports`

**matematika.js**

```js
exports.add = function (a, b) {
  return a + b;
}

exports.sub = function (a, b) {
  return a - b;
}

exports.mul = function (a, b) {
  return a * b;
}

exports.div = function (a, b) {
  return a / b;
}
```

**app.js**

```js
const matematika = require('./matematika');

const result = matematika.add(4, 2);
console.log(result);
```

### Menggunakan `module.exports`

**matematika.js**

```js
function add(a, b) {
  return a + b;
}

function sub(a, b) {
  return a - b;
}

function mul(a, b) {
  return a * b;
}

function div(a, b) {
  return a / b;
}

module.exports = {
  add,
  sub,
  mul,
  div
};
```

## Events

Setiap action pada komputer memberikan suatu event. Pada Node.js, kita dimungkinkan untuk membuat event kita sendiri.

**app.js**

```js
const EventEmitter = require('events');

class Button extends EventEmitter {
  constructor() {
    super()
    this.times = 0;
  }
}

const buttonA = new Button();

buttonA.on('click', (sound) => {
  sound ? console.log(sound) : {};
  buttonA.times++;
  console.log(`clicked ${buttonA.times} times`);
});

buttonA.emit('click');
buttonA.emit('click', 'beep');
buttonA.emit('click', 'boop');
```

### Event yang hanya satu kali dieksekusi

**app.js**

```js
const EventEmitter = require('events');

class Button extends EventEmitter {
  constructor() {
    super()
    this.times = 0;
  }
}

const buttonA = new Button();

buttonA.once('click', (sound) => {
  sound ? console.log(sound) : {};
  buttonA.times++;
  console.log(`clicked ${buttonA.times} times`);
});

buttonA.emit('click');
buttonA.emit('click', 'beep');
buttonA.emit('click', 'boop');
```

### Mengirim event error

**app.js**

```js
const EventEmitter = require('events');

class Button extends EventEmitter {
  constructor() {
    super()
    this.times = 0;
  }
}

const buttonA = new Button();

buttonA.once('click', (sound) => {
  sound ? console.log(sound) : {};
  buttonA.times++;
  console.log(`clicked ${buttonA.times} times`);
});

buttonA.emit('error', new Error('something wrong happened'));
console.log('checkpoint');
```

### Menangani event error

**app.js**

```js
const EventEmitter = require('events');

class Button extends EventEmitter {
  constructor() {
    super()
    this.times = 0;
  }
}

const buttonA = new Button();

buttonA.once('click', (sound) => {
  sound ? console.log(sound) : {};
  buttonA.times++;
  console.log(`clicked ${buttonA.times} times`);
});

buttonA.on('error', (error) => {
  console.log(error);
});

buttonA.emit('error', new Error('something wrong happened'));
console.log('checkpoint');
```

## Buffers

Pada saat kita melakukan proses transfer file, kita tidak dapat mengendalikan kecepatan pengiriman datanya. Kita hanya dapat mengetahui apakah data sudah diterima atau tidak. Node.js memiliki fitur buffer yang digunakan sebagai penampungan data sementara. Buffer hampir sama seperti array of integers namun dengan size yang tidak bisa berubah-ubah. Setiap angka pada buffer merepresentasikan sebuah byte sehingga nilainya berada dalam rentang 0-255.

**app.js**

```js
// Creating a buffer
const buffer = Buffer.alloc(20);

// Writing to a buffer
buffer.write('JavaScript');

// Reading the content of a buffer
console.log('Buffer:', buffer);

// Get buffer size
console.log('Length:', buffer.length);

// Reading the content of a buffer as string
console.log('Content as string:', buffer.toString());

// Looping through a buffer
for (const entry of buffer.entries()) {
  console.log(entry[1]);
}

// Writing to a buffer by indexing
for (let i = 0; i < 10; i++) {
  buffer[i] = i + 97;
}
console.log('Content as string:', buffer.toString());
```

## Streams

Pada saat kita melakukan transfer file dalam ukuran yang besar, maka tidak mungkin komputer kita menerima file tersebut secara utuh dalam suatu waktu. Hal ini dikarenakan ukuran dari memori komputer umumnya kecil. Berdasarkan hal tersebut lahirlah konsep mengenai stream. Stream merupakan aliran data yang dapat diibaratkan seperti aliran air. Agar kita tidak kewalahan saat menampung air yang banyak, kita dapat memberikan kendali pada aliran air tersebut. Kita dapat memasang kran untuk mengatur kapan air tersebut diijinkan untuk mengalir.

Pada Node.js, kita dapat membuat stream untuk membaca file menggunakan `fs.createReadStream()`. Selain stream untuk membaca file, kita juga dapat membuat stream untuk menulis ke sebuah file menggunakan `fs.createWriteStream()`. Salah satu implementasi dari buffer pada Node.js terdapat pada stream. Contoh sederhana dari penggunaan `fs.createReadStream()` dan `fs.createWriteStream()` adalah seperti di bawah. 

**app.js**

```js
const fs = require('fs');

const readStream = fs.createReadStream('./alice-in-wonderland.txt');
const writeStream = fs.createWriteStream('./result.txt');

readStream.on('data', (chunk) => {
  writeStream.write(chunk);
});

readStream.on('end', () => {
  console.log('stream closed');
});
```

File text yang digunakan pada contoh di atas dapat diunduh di [alice_in_wonderland.txt](https://gist.github.com/phillipj/4944029).

Contoh di atas mengesampingkan kejadian **backpressure** yang mungkin terjadi apabila melakukan proses pada file yang besar. Solusi yang baik untuk case di atas adalah menggunakan `.pipe()`, karena method tersebut akan menangani aliran data secara otomatis.

**app.js**

```js
const fs = require('fs');

const readStream = fs.createReadStream('./alice-in-wonderland.txt');
const writeStream = fs.createWriteStream('./result.txt');

readStream.pipe(writeStream);

readStream.on('end', () => {
  console.log('finished');
});
```

Informasi lebih lanjut mengenai backpressure dapat ditemukan di [sini](https://nodejs.org/en/docs/guides/backpressuring-in-streams/).

## File system

Node.js memiliki module bawaan yaitu `fs` yang dapat digunakan untuk keperluan pengolahan file. Referensi lengkap mengenai module ini dapat ditemukan di [sini](https://nodejs.org/docs/latest-v16.x/api/fs.html).

### Membaca file secara asynchronous

**home.html**

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <h1>Hello world!</h1>
    <p>Introduction to Node.js</p>
</body>

</html>
```

**app.js**

```js
const http = require('http');
const fs = require('fs');

const server = http.createServer(function (req, res) {
  fs.readFile('./home.html', (err, data) => {
    if (err) throw err;
    res.writeHead(200, {
      'Content-Type': 'text/html'
    });
    res.write(data);
    res.end();
  });
});

server.listen(3000);
```

### Membaca file secara asynchronous (promise)

**app.js**

```js
const http = require('http');
const fs = require('fs/promises');

const server = http.createServer(async function (req, res) {
  try {
    const data = await fs.readFile('./home.html');
    res.writeHead(200, {
      'Content-Type': 'text/html'
    });
    res.write(data);
    res.end();
  } catch (err) {
    throw err;
  }
});

server.listen(3000);
```

### Membuat file dibarengi dengan menuliskan sesuatu

**app.js**

```js
const fs = require('fs');

fs.appendFile('./test.txt', 'this is a test', (err) => {
  if (err) throw err;
  console.log('done');
});
```

### Membuat file dan melakukan operasi read dan write

**app.js**

```js
const fs = require('fs');

fs.open('./test.txt', 'w+', (err, fd) => {
  if (err) throw err;
  fs.write(fd, 'this is a test', (err) => {
    if (err) throw err;
    fs.read(fd, { position: 0 }, (err, bytesRead, buffer) => {
      if (err) throw err;
      console.log('Bytes read:', bytesRead);
      console.log('Content:', buffer.toString());
    });
  });
});
```

### Mengubah nama file

**app.js**

```js
const fs = require('fs');

fs.rename('./test.txt', './coba.txt', (err) => {
  if (err) throw err;
  console.log('done');
});
```

### Menghapus file

**app.js**

```js
const fs = require('fs');

fs.unlink('./coba.txt', (err) => {
  if (err) throw err;
  console.log('done');
});
```

## CLI

Selain digunakan untuk mengembangkan aplikasi back-end, Node.js juga dapat digunakan untuk membuat aplikasi CLI (Command Line Interface). Terdapat bayak package dari komunitas Node.js yang dapat digunakan untuk memudahkan kita dalam membuat aplikasi berbasis CLI, salah satunya adalah [commander](https://www.npmjs.com/package/commander). Contoh penggunaan sederhana dari commander adalah seperti di bawah.

External modules yang digunakan:

- [commander](https://www.npmjs.com/package/commander)
- [Axios](https://www.npmjs.com/package/axios)
- [CLI Table](https://www.npmjs.com/package/cli-table)

**app.js**

```js
const { program } = require('commander');
const axios = require('axios');
const Table = require('cli-table');

program
  .name('jsonplaceholder-util')
  .description('CLI to get data from JSONPlaceholder')
  .version('0.0.1');

program
  .command('fetch-users')
  .description('Fetch users data from JSONPlaceholder')
  .action(async () => {
    try {
      const { data } = await axios({
        method: 'GET',
        url: 'https://jsonplaceholder.typicode.com/users'
      });
      const table = new Table({
        head: ['Name', 'Username', 'Email']
      });
      data.forEach((user) => {
        table.push([user.name, user.username, user.email]);
      });
      console.log(table.toString());
    } catch (err) {
      throw err;
    }
  });

program.parse(process.argv);
```
