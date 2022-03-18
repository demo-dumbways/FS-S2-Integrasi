---
sidebar_position: 5
---

# 5. Fetching

import useBaseUrl from '@docusaurus/useBaseUrl';

Berikut langkah-langkah mengambil data dari `Server`:

### 5.1 Product (Customer)

> Path: `client/src/pages/Product.js`

- Import `useQuery` dari package `react-query`

  ```js
  import { useQuery } from 'react-query';
  ```

- Ambil konfigurasi function `API`

  ```js
  import { API } from '../config/api';
  ```

- Fetching data products dari endpoint `/products`

  ```js
  let { data: products } = useQuery('productsCache', async () => {
    const response = await API.get('/products');
    return response.data.data;
  });
  ```

  Data akan tersimpan didalam variable `products`

  Referensi: [useQuery](https://react-query.tanstack.com/reference/useQuery)

- Render

  Untuk menampilkan data yang diperoleh dari `API backend` kita dapat menggunakan function `MAP`

  ```jsx {2,5,7,10-12}
  <>
    {products?.map((item, index) => (
      <div className="card-product mt-3">
        <img
          src={item.image}
          className="img-fluid img-rounded"
          alt={item.name}
        />
        <div className="p-2">
          <div className="text-header-product-item">{item.name}</div>
          <div className="text-product-item">{item.price}</div>
          <div className="text-product-item">Stock : {item.qty}</div>
        </div>
      </div>
    ))}
  </>
  ```

- Klik tombol dibawah ini untuk melihat `full code`

    <a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-results-stage-2-integration-frontend/blob/main/src/pages/Product.js">
    Contoh Code
    </a>

- Demo

  <img alt="image1-2" src={useBaseUrl('img/docs/image-integration-5.png')} width="100%"/>

  <br />
  <br />

    <a class="btn-demo" href="https://ebook-code-results-stage-2-integration-frontend-ruby.vercel.app/">
    Demo
    </a>

    Silakan login menggunakan akun `customer` dibawah ini:
    - email: `customer@mail.com`
    - password: `123456`

### 5.2 Product (Admin)

> Path: `client/src/pages/ProductAdmin.js`

- Klik tombol dibawah ini untuk melihat `full code`

    <a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-results-stage-2-integration-frontend/blob/main/src/pages/ProductAdmin.js">
    Contoh Code
    </a>

- Demo

  <img alt="image1-2" src={useBaseUrl('img/docs/image-integration-8.png')} width="100%"/>

  <br />
  <br />

    <a class="btn-demo" href="https://ebook-code-results-stage-2-integration-frontend-ruby.vercel.app/product-admin">
    Demo
    </a>

    Silakan login menggunakan akun `admin` dibawah ini:
    - email: `admin@mail.com`
    - password: `123456`

### 5.3 Detail Product

> Path: `client/src/pages/DetailProduct.js`

- Klik tombol dibawah ini untuk melihat `full code`

    <a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-results-stage-2-integration-frontend/blob/main/src/pages/DetailProduct.js">
    Contoh Code
    </a>

- Demo

  <img alt="image1-2" src={useBaseUrl('img/docs/image-integration-6.png')} width="100%"/>

  <br />
  <br />

    <a class="btn-demo" href="https://ebook-code-results-stage-2-integration-frontend-ruby.vercel.app/product/2">
    Demo
    </a>

    Silakan login menggunakan akun `customer` dibawah ini:
    - email: `customer@mail.com`
    - password: `123456`

### 5.5 Profile & Transaction

> Path: `client/src/pages/Profile.js`

- Klik tombol dibawah ini untuk melihat `full code`

    <a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-results-stage-2-integration-frontend/blob/main/src/pages/Profile.js">
    Contoh Code
    </a>

- Demo

  <img alt="image1-2" src={useBaseUrl('img/docs/image-integration-7.png')} width="100%"/>

  <br />
  <br />

    <a class="btn-demo" href="https://ebook-code-results-stage-2-integration-frontend-ruby.vercel.app/profile">
    Demo
    </a>

    Silakan login menggunakan akun `customer` dibawah ini:
    - email: `customer@mail.com`
    - password: `123456`

### 5.5 Category

> Path: `client/src/pages/CategoryAdmin.js`

- Klik tombol dibawah ini untuk melihat `full code`

    <a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-results-stage-2-integration-frontend/blob/main/src/pages/CategoryAdmin.js">
    Contoh Code
    </a>

- Demo

  <img alt="image1-2" src={useBaseUrl('img/docs/image-integration-9.png')} width="100%"/>

  <br />
  <br />

    <a class="btn-demo" href="https://ebook-code-results-stage-2-integration-frontend-ruby.vercel.app/category-admin">
    Demo
    </a>

    Silakan login menggunakan akun `admin` dibawah ini:
    - email: `admin@mail.com`
    - password: `123456`
