---
sidebar_position: 4
---

# 4. Handling Login with useContext

import useBaseUrl from '@docusaurus/useBaseUrl';

**useContext** dapat di katakan sebagai sebuah `general store`, dengan begitu `data` dapat kita gunakan di berbagai macam `component`, tanpa harus menjadikan `data` sebagai `props` di setiap komponennya.

Oleh karena itu kita perlu `useContext` untuk membuat `sistem` yang dapat mengecek apakah `user` tersebut telah `login` ke `sistem` dan jika browser di `refresh` status user akan `tetap login` disetiap komponent.

## 4.1 Server Side

Pada sisi `Server` kita perlu membuat sebuah `endpoint` yang dapat menangani pengecekan `token` yang dikirim dari `client`.


<a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-results-stage-2-integration-backend/blob/main/src/controllers/auth.js">
    Contoh Code
</a>

<br />
<br />

```js {4,6-13,15-19,24-29} title=src/controllers/auth.js
exports.checkAuth = async (req, res) => {
  try {
    // Get id from token
    const id = req.user.id;

    const dataUser = await user.findOne({
      where: {
        id,
      },
      attributes: {
        exclude: ['createdAt', 'updatedAt', 'password'],
      },
    });

    if (!dataUser) {
      return res.status(404).send({
        status: 'failed',
      });
    }

    res.send({
      status: 'success...',
      data: {
        user: {
          id: dataUser.id,
          name: dataUser.name,
          email: dataUser.email,
          status: dataUser.status,
        },
      },
    });
  } catch (error) {
    console.log(error);
    res.status({
      status: 'failed',
      message: 'Server Error',
    });
  }
};
```


## 4.2 Client Side

- Pada file `App.js`, tambahkan code berikut:

  > Path: `client/src/App.js`

  - Import `useContext` & `UserContext`

    ```js
    import { useContext } from 'react';

    import { UserContext } from './context/userContext';
    ```

  - Import function `API` & `setAuthToken`

    ```js
    import { API, setAuthToken } from './config/api';
    ```

  - Cek token & simpan dalam `Headers authorization`

    ```js
    if (localStorage.token) {
      setAuthToken(localStorage.token);
    }
    ```

  - Init user context

    ```js
    const [state, dispatch] = useContext(UserContext);
    ```

  - Redirect Auth

    ```js
    useEffect(() => {
      // Redirect Auth
      if (state.isLogin == false) {
        history.push('/auth');
      } else {
        if (state.user.status == 'admin') {
          history.push('/complain-admin');
        } else if (state.user.status == 'customer') {
          history.push('/');
        }
      }
    }, [state]);
    ```

  - Check user token

    ```js
    const checkUser = async () => {
      try {
        const response = await API.get('/check-auth');

        // If the token incorrect
        if (response.status === 404) {
          return dispatch({
            type: 'AUTH_ERROR',
          });
        }

        // Get user data
        let payload = response.data.data.user;
        // Get token from local storage
        payload.token = localStorage.token;

        // Send data to useContext
        dispatch({
          type: 'USER_SUCCESS',
          payload,
        });
      } catch (error) {
        console.log(error);
      }
    };

    useEffect(() => {
      checkUser();
    }, []);
    ```

  - Klik tombol dibawah ini untuk melihat `full code`

    <a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-results-stage-2-integration-frontend/blob/main/src/App.js">
          Contoh Code
      </a>

      <br />
      <br />
      <br />



- Pada file `userContext.js`, tambahkan code berikut:

  > Path: `client/src/context/userContext.js`

  - Import `createContext` & `useReducer`

    ```js
    import { createContext, useReducer } from 'react';
    ```

  - Import `createContext`

    ```js
    export const UserContext = createContext();
    ```

  - Deklarasi State

    ```js
    const initialState = {
      isLogin: false,
      user: {},
    };
    ```

  - Function `reducer`

    ```js
    const reducer = (state, action) => {
      const { type, payload } = action;

      switch (type) {
        case 'USER_SUCCESS':
        case 'LOGIN_SUCCESS':
          localStorage.setItem('token', payload.token);
          return {
            isLogin: true,
            user: payload,
          };
        case 'AUTH_ERROR':
        case 'LOGOUT':
          localStorage.removeItem('token');
          return {
            isLogin: false,
            user: {},
          };
        default:
          throw new Error();
      }
    };
    ```

  - Function `UserContextProvider`

    ```jsx
    export const UserContextProvider = ({ children }) => {
      const [state, dispatch] = useReducer(reducer, initialState);

      return (
        <UserContext.Provider value={[state, dispatch]}>
          {children}
        </UserContext.Provider>
      );
    };
    ```
  - Klik tombol dibawah ini untuk melihat `full code`

    <a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-results-stage-2-integration-frontend/blob/main/src/context/userContext.js">
        Contoh Code
    </a>
