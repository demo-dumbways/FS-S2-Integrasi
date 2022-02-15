---
sidebar_position: 2
---

# 2. Hashing Password Bcrypt

import useBaseUrl from '@docusaurus/useBaseUrl';

**Bcrypt** merupakan package untuk mengamankan data password kita agar datanya tidak diketahui orang lain dengan cara dienkrip passwordnya, enkripsi akan menjamin data-data tetap aman meskipun berada di tangan orang lain, karena mereka tidak tahu isi asli dari datanya.

contoh password belum di enkrip
```123456```

contoh password sudah di enkrip
```$2a$10$BLMmNP5ERgFYq3HZEH2b9.2uZG6qYrUbDbPx8GGluNGWbPz7/Oh72```

Sebelumnya kita install terlebih dahulu dengan perintah.

```shell
npm install bcrypt
```

Selanjutnya kita implementasikan, berikut contoh codenya:

<a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-results-stage-2-backend/blob/1-expressjs-fundamental/index.js">
Contoh code
</a>

<br />
<br />

```js title=controllers/auth.js
const { user } = require("../../models");

const Joi = require("joi");
const bcrypt = require("bcrypt");

exports.register = async (req, res) => {
  const schema = Joi.object({
    name: Joi.string().min(5).required(),
    email: Joi.string().email().min(6).required(),
    password: Joi.string().min(6).required(),
  });

  const { error } = schema.validate(req.body);

  if (error)
    return res.status(400).send({
      error: {
        message: error.details[0].message,
      },
    });

  try {

    const salt = await bcrypt.genSalt(10);
    const hashedPassword = await bcrypt.hash(req.body.password, salt);

    const newUser = await user.create({
      name: req.body.name,
      email: req.body.email,
      password: hashedPassword,
    });

    res.status(200).send({
      status: "success...",
      data: {
        name: newUser.name,
        email: newUser.email,
      },
    });
  } catch (error) {
    console.log(error);
    res.status(500).send({
      status: "failed",
      message: "Server Error",
    });
  }
};

exports.login = async (req, res) => {
  const schema = Joi.object({
    email: Joi.string().email().min(6).required(),
    password: Joi.string().min(6).required(),
  });

  const { error } = schema.validate(req.body);

  if (error)
    return res.status(400).send({
      error: {
        message: error.details[0].message,
      },
    });

  try {
    const userExist = await user.findOne({
      where: {
        email: req.body.email,
      },
      attributes: {
        exclude: ["createdAt", "updatedAt"],
      },
    });

    const isValid = await bcrypt.compare(req.body.password, userExist.password);

    if (!isValid) {
      return res.status(400).send({
        status: "failed",
        message: "credential is invalid",
      });
    }
    res.status(200).send({
      status: "success...",
      data: {
        name: userExist.name,
        email: userExist.email
      },
    });
  } catch (error) {
    console.log(error);
    res.status(500).send({
      status: "failed",
      message: "Server Error",
    });
  }
};
```

<img alt="image1-2" src={useBaseUrl('img/docs/image-4-1.png')} width="60%"/>

<br />
<br />

<div>
<a class="btn-demo" href="https://ebook-code-results-stage-2-backend-git-1-e-bef277-demo-dumbways.vercel.app/">
Demo
</a>
</div>