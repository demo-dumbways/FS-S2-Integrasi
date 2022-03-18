---
sidebar_position: 2
---

# 2. Prepare

import useBaseUrl from '@docusaurus/useBaseUrl';

Sebelum melakukan `Integration`, kita perlu melakukan beberapa persiapan seperti mengatur `struktur folder`, `installasi package`, dan `refactor code`.

## 2.1 Struktur Folder

Buatlah sebuah folder yang akan menambung aplikasi `Frontend` (Client) dan `Backend` (Server), seperti berikut:

<a class="btn-example-code" href="https://github.com/demo-dumbways/-ebook-code-results-stage-2-integration">
Contoh Folder
</a>

<br />
<br />

```text {2,3}
DumbGram Integration
┣ client
┗ server
```

Folder `client` berisikan aplikasi `ReactJs` (Frontend) dan Folder `Server` berisikan aplikasi `ExpressJS` (Backend)

## 2.2 Installasi Package

Kita perlu menginstall beberapa package yang akan digunakan untuk keperluan proses `Integration`

### Package for `Server Side`

- Concurrently
  ```bash
  npm i concurrently
  ```
- CORS

  ```bash
  npm i cors
  ```

### Package for `Client Side`

- Axios

  A promise based HTTP client for the browser and Node.js

  ```bash
  npm install axios
  ```

  For more info about `Axios` please refer to this [Link](https://axios-http.com/docs/intro)

- React Query

  React query is a collection of hooks for fetching, caching, and updating asynchronous state in React.

  ```bash
  npm i react-query
  ```

  For more info about `React-Query` please refer to this [Link](https://react-query.tanstack.com/overview)

## 2.3 Code

Kita juga perlu menambahkan beberapa code dibawah ini, baik pada `Server Side` dan `Client Side`

### Code for `Server Side`

- Tambahkan code dibawah ini pada file `package.json`

  <a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-results-stage-2-integration-backend/blob/main/package.json">
  Contoh Code
  </a>

  <br />
  <br />

  ```json title=server/package.json
  "scripts": {
    "start": "nodemon index.js",
    "client": "npm start --prefix ../client",
    "dev": "concurrently \"npm start\" \"npm run client\""
  },
  ```

  Code diatas bertujuan jika kita ingin menjalankan kedua aplikasi (Client & Server) secara `bersamaan`, maka kita dapat menggunakan perintah:

  ```bash
  npm run dev
  ```

- Tambahkan code dibawah ini pada file `index.js`

  <a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-results-stage-2-integration-backend/blob/main/index.js">
  Contoh Code
  </a>

  <br />
  <br />

  ```js {1,3,5} title=server/index.js
  const cors = require('cors');

  const port = process.env.PORT || 5000;

  app.use(cors());
  ```


  Code diatas adalah cara memasang `cors` pada aplikasi `backend` dan mengambil value `PORT` dari `server hosting` jika kita telah melakukan `deploy`.

### Code for `Client Side`

- Tambahkan code dibawah ini pada file `api.js`

  <a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-results-stage-2-integration-frontend/blob/main/src/config/api.js">
  Contoh Code
  </a>

  <br />
  <br />

  ```js {1,3-6,8-14} title=client/src/config/api.js
  import axios from 'axios';

  export const API = axios.create({
    baseURL:
      process.env.REACT_APP_SERVER_URL || 'http://localhost:5000/api/v1/',
  });

  export const setAuthToken = (token) => {
    if (token) {
      API.defaults.headers.common['Authorization'] = `Bearer ${token}`;
    } else {
      delete API.defaults.headers.commin['Authorization'];
    }
  };
  ```


  Code diatas terdapat 2 function, Function `API` untuk setup baseURL atau `Server Url` agar `client` dapat mengetahui `server` yang akan dituju, `Server Url` dapat diambil dari `Environment Variable` `REACT_APP_SERVER_URL` jika telah melakukan `deploy`. Function `setAuthToken` bertujuan agar dapat menyimpan token kedalam `headers`.

- Tambahkan code dibawah ini pada file `index.js`

  Import QueryClient and QueryClientProvider :

  ```js
  import { QueryClient, QueryClientProvider } from 'react-query';
  ```

  Init Client and set QueryClientProvider:

  <a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-results-stage-2-integration-frontend/blob/main/src/index.js">
  Contoh Code
  </a>

  <br />
  <br />

  ```jsx {1,6,10}
  const client = new QueryClient();

  ReactDOM.render(
    <React.StrictMode>
      <UserContextProvider>
        <QueryClientProvider client={client}>
          <Router>
            <App />
          </Router>
        </QueryClientProvider>
      </UserContextProvider>
    </React.StrictMode>,
    document.getElementById('root')
  );
  ```


  Menggunakan component `QueryClientProvider` untuk `menghubungkan` dan `menyediakan` `QueryClient` ke aplikasi.
