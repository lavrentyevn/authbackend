# Authentication on Express JS using JWT

This is a backend application that implements:

## Registration
You can send a JSON object consisting of a username and a password to a non-protected API, which will check if there is already a user with such a username in a database and, if not, save your data and send a 200 status message.

<img width="775" alt="Снимок экрана 2023-11-13 в 12 05 12" src="https://github.com/lavrentyevn/authbackend/assets/111048277/bb559b5b-98e3-4134-a1d6-03df035e64ff">

It is important to note that passwords are **not** stored in a decoded state. Any password is stored as a **hash**, which can be obtained by a password hashing algorithm, such as **bcrypt**.

<img width="543" alt="Снимок экрана 2023-11-13 в 12 47 03" src="https://github.com/lavrentyevn/authbackend/assets/111048277/f35767ed-8796-462b-9947-07dc757277e1">


## Authentication
You can send a JSON object consisting of a username and a password to a non-protected API, which will check if there is a user with such a username in a database, then it will use **bcrypt** to compare a decoded password with a hashed password. Finally, the server will send two types of **jwt** tokens.

-  Access Token <br/>
This token is necessary for accessing any protected route. This token is not stored in a database; it is sent as a JSON object, which can be used later as a Bearer Token. It also has an expiration time, which is usually short (< 15 min).

<img width="772" alt="Снимок экрана 2023-11-13 в 12 15 03" src="https://github.com/lavrentyevn/authbackend/assets/111048277/e7753d5b-604a-47f1-97ac-862ea4bc0d58">

-  Refresh Token <br/>
This token is necessary for creating a new access token after it has expired (or been lost). This token is stored in a database; it is sent as a cookie, which has the following options:

httpOnly:true <br/>
This is important as it makes our cookies only accessible through http requests, which means that it cannot be obtained through JavaScript.
 
secure:true <br/>
It makes our cookies only work with secure routes (https).

sameSite:"none" <br/>
It makes cookies work with both cross-site and same-site requests.

maxAge: 24 * 60 * 60 * 1000 <br/>
Cookie must also have an expiration time.

<img width="770" alt="Снимок экрана 2023-11-13 в 12 23 59" src="https://github.com/lavrentyevn/authbackend/assets/111048277/04b17967-e34e-473c-a44c-80982c97d2ed">

## Refresh
Access tokens have a short expiration time. Refresh tokens allow us to create a new access token, which can be used to access protected routes. It is important to note that we do not have to send a JSON object consisting of user data to this request. We send a cookie that has a refresh token.

<img width="771" alt="Снимок экрана 2023-11-13 в 12 16 20" src="https://github.com/lavrentyevn/authbackend/assets/111048277/5fa595c5-881d-4e74-b768-e784fe759f1e">

## Logout
Logging out deletes our refresh token (a cookie that has a refresh token), which prevents us from creating new access tokens.

<img width="768" alt="Снимок экрана 2023-11-13 в 12 24 21" src="https://github.com/lavrentyevn/authbackend/assets/111048277/36879d5f-454a-4c7b-8282-cf45d8736018">

### User data is stored in MongoDB
As an example, we can send a request to a protected api, which checks the access token.

<img width="554" alt="Снимок экрана 2023-11-13 в 13 11 09" src="https://github.com/lavrentyevn/authbackend/assets/111048277/533543f3-fb88-4dcc-b913-d2a51894daa5">

If the token is valid, we can access data.

<img width="766" alt="Снимок экрана 2023-11-13 в 12 15 23" src="https://github.com/lavrentyevn/authbackend/assets/111048277/7e8c73fa-499e-48f7-aca0-4bd698cdd9db">

The same data in MongoDB.

<img width="724" alt="Снимок экрана 2023-11-13 в 13 15 09" src="https://github.com/lavrentyevn/authbackend/assets/111048277/0396e7a6-e8fd-45bc-8563-e475f0d97ede">
