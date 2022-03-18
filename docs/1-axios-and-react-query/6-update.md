---
sidebar_position: 6
---

# 6. Update

import useBaseUrl from '@docusaurus/useBaseUrl';

Berikut langkah-langkah mengupdate data:

### 6.1 Product

`Login as Admin`

> Path: `client/src/pages/UpdateProductAdmin.js`

- Import `useQuery` & `useMutation` dari package `react-query`

  ```js
  import { useQuery, useMutation } from 'react-query';
  ```

- Ambil konfigurasi function `API`

  ```js
  import { API } from '../config/api';
  ```

- Deklarasi `useState` untuk menyimpan data `product` dan `form`

  ```js
  const [product, setProduct] = useState({});
  const [form, setForm] = useState({
    image: '',
    name: '',
    desc: '',
    price: '',
    qty: '',
  });
  ```

- Fetching sebuah data product dari endpoint `/products/:id` berdasarkan `id` dan simpan kedalam variabel useState

  ```js
  useQuery('productCache', async () => {
    const response = await API.get('/product/' + id);
    setForm({
      ...form,
      name: response.data.data.name,
      desc: response.data.data.desc,
      price: response.data.data.price,
      qty: response.data.data.qty,
    });
    setProduct(response.data.data);
  });
  ```

- Render

  Data yang diperoleh dari `API backend` akan ditampilkan dalam component `Input`

  ```jsx {6}
  <input
    type="text"
    placeholder="Product Name"
    name="name"
    onChange={handleChange}
    value={form?.name}
    className="input-edit-category mt-4"
  />
  ```

- Function `handleSubmit`

  ```js {1,6-10,13-21,24-28}
  const handleSubmit = useMutation(async (e) => {
    try {
      e.preventDefault();

      // Configuration
      const config = {
        headers: {
          'Content-type': 'multipart/form-data',
        },
      };

      // Store data with FormData as object
      const formData = new FormData();
      if (form.image) {
        formData.set('image', form?.image[0], form?.image[0]?.name);
      }
      formData.set('name', form.name);
      formData.set('desc', form.desc);
      formData.set('price', form.price);
      formData.set('qty', form.qty);
      formData.set('categoryId', categoryId);

      // Insert product data
      const response = await API.patch(
        '/product/' + product.id,
        formData,
        config
      );
    } catch (error) {
      console.log(error);
    }
  });
  ```

  Code diatas merupakan proses pengiriman data telah diubah oleh `User`, dimana data dikirim melalui endpoint `/product/:id`, `id` disini merupakan `id` dari product yang akan diubah datanya.

- Klik tombol dibawah ini untuk melihat `full code`

    <a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-results-stage-2-integration-frontend/blob/main/src/pages/AddProductAdmin.js">
    Contoh Code
    </a>

- Demo

  <img alt="image1-2" src={useBaseUrl('img/docs/image-integration-10.png')} width="100%"/>

  <br />
  <br />

    <a class="btn-demo" href="https://ebook-code-results-stage-2-integration-frontend-ruby.vercel.app/update-product/2">
    Demo
    </a>

    Silakan login menggunakan akun `admin` dibawah ini:
    - email: `admin@mail.com`
    - password: `123456`

### 6.2 Category

`Login as Admin`

> Path: `client/src/pages/UpdateCategoryAdmin.js`

- Klik tombol dibawah ini untuk melihat `full code`

    <a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-results-stage-2-integration-frontend/blob/main/src/pages/UpdateCategoryAdmin.js">
    Contoh Code
    </a>

- Demo

  <img alt="image1-2" src={useBaseUrl('img/docs/image-integration-11.png')} width="100%"/>

  <br />
  <br />

    <a class="btn-demo" href="https://ebook-code-results-stage-2-integration-frontend-ruby.vercel.app/category-admin">
    Demo
    </a>

    Silakan login menggunakan akun `admin` dibawah ini:
    - email: `admin@mail.com`
    - password: `123456`
