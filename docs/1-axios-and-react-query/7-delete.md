---
sidebar_position: 7
---

# 7. Delete

import useBaseUrl from '@docusaurus/useBaseUrl';

Berikut langkah-langkah menghapus data:

### 7.1 Product

`Login as Admin`

> Path: `client/src/pages/Product.js`

- Import `useQuery` & `useMutation` dari package `react-query`

  ```js
  import { useQuery, useMutation } from 'react-query';
  ```

- Deklarasi `useState` untuk menyimpan data `idProduct` dan `confirmDelete`

  ```js
  const [idDelete, setIdDelete] = useState(null);
  const [confirmDelete, setConfirmDelete] = useState(null);
  ```

- Deklarasi `useState` untuk menyimpan data `boolean` modal dan `penanganannya`

  ```js
  const [show, setShow] = useState(false);
  const handleClose = () => setShow(false);
  const handleShow = () => setShow(true);
  ```

- Function untuk mengambil id product & show modal confirm delete data ketika tombol `delete` ditekan oleh user

  ```js
  const handleDelete = (id) => {
    setIdDelete(id);
    handleShow();
  };
  ```

- Jika user mengkonfirmasi product yakin dihapus (`boolean: true`), maka jalankan function `deleteById`

  ```js
  const deleteById = useMutation(async (id) => {
    try {
      await API.delete(`/product/${id}`);
      refetch();
    } catch (error) {
      console.log(error);
    }
  });
  ```

- Jika terdapat perubahan data pada variable `confirmDelete` maka Lifecycle `Did Update` akan terpicu sehingga akan menutup modal dan menjalankan function `deleteById.mutate(idDelete)`

  ```js
  useEffect(() => {
    if (confirmDelete) {
      handleClose();
      deleteById.mutate(idDelete);
      setConfirmDelete(null);
    }
  }, [confirmDelete]);
  ```

- Klik tombol dibawah ini untuk melihat `full code`

    <a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-results-stage-2-integration-frontend/blob/main/src/pages/ProductAdmin.js">
    Contoh Code
    </a>

- Demo

  <img alt="image1-2" src={useBaseUrl('img/docs/image-integration-12.png')} width="100%"/>

  <br />
  <br />

    <a class="btn-demo" href="https://ebook-code-results-stage-2-integration-frontend-ruby.vercel.app/product-admin">
    Demo
    </a>

    Silakan login menggunakan akun `admin` dibawah ini:
    - email: `admin@mail.com`
    - password: `123456`

### 7.2 Category

`Login as Admin`

> Path: `client/src/pages/UpdateCategoryAdmin.js`

- Klik tombol dibawah ini untuk melihat `full code`

    <a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-results-stage-2-integration-frontend/blob/main/src/pages/CategoryAdmin.js">
    Contoh Code
    </a>

- Demo

  <img alt="image1-2" src={useBaseUrl('img/docs/image-integration-13.png')} width="100%"/>

  <br />
  <br />

    <a class="btn-demo" href="https://ebook-code-results-stage-2-integration-frontend-ruby.vercel.app/category-admin">
    Demo
    </a>

    Silakan login menggunakan akun `admin` dibawah ini:
    - email: `admin@mail.com`
    - password: `123456`
