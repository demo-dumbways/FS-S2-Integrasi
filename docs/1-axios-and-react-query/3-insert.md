---
sidebar_position: 3
---

# 3. Insert

import useBaseUrl from '@docusaurus/useBaseUrl';

Berikut langkah-langkah mengirim data ke `Server`:

## 3.1 Register

> Path: `client/src/components/auth/Register.js`

- Import `useMutation` dari package `react-query` :

  ```js
  import { useMutation } from "react-query";
  ```

- Ambil konfigurasi `baseURL` dari function `API`

  ```js
  import { API } from "../../config/api";
  ```

- Deklarsi `useState` untuk menyimpan data dari form

  ```js {1-5,7}
  const [form, setForm] = useState({
    name: "",
    email: "",
    password: "",
  });

  const { name, email, password } = form;
  ```

- Buat function untuk menangani perubahan data

  ```js
  const handleChange = (e) => {
    setForm({
      ...form,
      [e.target.name]: e.target.value,
    });
  };
  ```

- Buat function untuk menangani proses pengiriman data ke `Server`

  ```js
  const handleSubmit = useMutation(async (e) => {
    try {
      e.preventDefault();

      //Konfigurasi
      const config = {
        headers: {
          "Content-type": "application/json",
        },
      };

      //Convert data menjadi string
      const body = JSON.stringify(form);

      //Mengirim data ke Server melalui API endpoint '/register'
      await API.post("/register", body, config);
    } catch (error) {
      console.log(error);
    }
  });
  ```

- Cara menjalankan function `handleSubmit` dengan menambahkan `.mutate`

  ```jsx
  <form onSubmit={(e) => handleSubmit.mutate(e)}>
  ```

- Klik tombol dibawah ini untuk melihat `full code`

    <a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-results-stage-2-integration-frontend/blob/main/src/components/auth/Register.js">
    Contoh Code
    </a>

- Demo

  <img alt="image1-2" src={useBaseUrl('img/docs/image-integration-1.png')} width="100%"/>

  <br />
  <br />

    <a class="btn-demo" href="https://ebook-code-results-stage-2-integration-frontend-ruby.vercel.app/auth">
    Demo
    </a>

## 3.2 Login

> Path: `client/src/components/auth/Login.js`

- Klik tombol dibawah ini untuk melihat `full code`

    <a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-results-stage-2-integration-frontend/blob/main/src/components/auth/Login.js">
    Contoh Code
    </a>

- Demo

  <img alt="image1-2" src={useBaseUrl('img/docs/image-integration-2.png')} width="100%"/>
  <br />
  <br />

  <a class="btn-demo" href="https://ebook-code-results-stage-2-integration-frontend-ruby.vercel.app/auth">
  Demo
  </a>

  Silakan `login` menggunakan akun yang telah Anda `daftarkan` atau gunakan akun dibawah ini:

  - Akun Admin:
    - email: `admin@mail.com`
    - password: `123456`
  - Akun Customer:
    - email: `customer@mail.com`
    - password: `123456`


## 3.3 Product

> Path: `client/src/pages/AddProductAdmin.js`

`Login as Admin`

Karena didalam data `product` terdapat data berupa `file` yaitu file `gambar product`, oleh karena itu perlu memasukkan data form kedalam [FormData object](https://developer.mozilla.org/en-US/docs/Web/API/FormData/Using_FormData_Objects) dan pada konfigurasi `Headers` atur value `Content-type` menjadi `multipart/form-data`.

- Code pada function `handleSubmit`

  ```js {6-10,13-19,22}
  const handleSubmit = useMutation(async (e) => {
    try {
      e.preventDefault();

      // Configuration
      const config = {
        headers: {
          "Content-type": "multipart/form-data",
        },
      };

      // Store data with FormData as object
      const formData = new FormData();
      formData.set("image", form.image[0], form.image[0].name);
      formData.set("name", form.name);
      formData.set("desc", form.desc);
      formData.set("price", form.price);
      formData.set("qty", form.qty);
      formData.set("categoryId", categoryId);

      // Insert product data
      const response = await API.post("/product", formData, config);
    } catch (error) {
      console.log(error);
    }
  });
  ```

- Klik tombol dibawah ini untuk melihat `full code`

    <a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-results-stage-2-integration-frontend/blob/main/src/pages/AddProductAdmin.js">
    Contoh Code
    </a>

- Demo

  <img alt="image1-2" src={useBaseUrl('img/docs/image-integration-3.png')} width="100%"/>

    <br />
  <br />

    <a class="btn-demo" href="https://ebook-code-results-stage-2-integration-frontend-ruby.vercel.app/add-product">
    Demo
    </a>

    Silakan login menggunakan akun `admin` dibawah ini:
    - email: `admin@mail.com`
    - password: `123456`

## 3.4 Category

> Path: `client/src/pages/AddCategoryAdmin.js`

`Login as Admin`

- Klik tombol dibawah ini untuk melihat `full code`

    <a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-results-stage-2-integration-frontend/blob/main/src/pages/AddCategoryAdmin.js">
    Contoh Code
    </a>

- Demo

  <img alt="image1-2" src={useBaseUrl('img/docs/image-integration-4.png')} width="100%"/>

    <br />
    <br />

    <a class="btn-demo" href="https://ebook-code-results-stage-2-integration-frontend-ruby.vercel.app/add-product">
    Demo
    </a>

    Silakan login menggunakan akun `admin` dibawah ini:
    - email: `admin@mail.com`
    - password: `123456`
