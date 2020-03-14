---
title: Sign in with apple
date: 2020-03-14
---

apple 이 제 3자 서비스 인증을 활용하는 app 에 대해서, apple 로 로그인 기능을 넣으라고 협박하고 있습니다.
지가 뭔데 남의 서비스에 감 놔라 배 놔라 하는지 모르겠습니다.

https://developer.apple.com/news/?id=09122019b

> We’ve updated the App Store Review Guidelines to provide criteria for
> when apps are required to use Sign in with Apple. Starting today, new
> apps submitted to the App Store must follow these guidelines. Existing
> apps and app updates must follow them by April 2020. We’ve also
> provided new guidelines for using Sign in with Apple on the web and
> other platforms.

https://developer.apple.com/app-store/review/guidelines/#sign-in-with-apple

> Apps that use a third-party or social login service (such as Facebook
> Login, Google Sign-In, Sign in with Twitter, Sign In with LinkedIn,
> Login with Amazon, or WeChat Login) to set up or authenticate the
> user’s primary account with the app must also offer Sign in with
> Apple as an equivalent option. A user’s primary account is the
> account they establish with your app for the purposes of identifying
> themselves, signing in, and accessing your features and associated
> services.


# 함정

어케 맨질일이 있어서 경험해봤는데 함정이 곳곳에 있습니다.
일반적인 OAuth provider 가 아닙니다.

함정은..

-   access\_token 으로 사용자 profile 을 얻어올 수 있는 REST API 가 없고(..못찾겠고)
-   **callback 이 POST 로 오고** (..scope 지정하면)
-   token 요청할때 `client_secret` 을 제공받은 key 파일로 암호화 해서 생성해야하고,
-   **사용자 프로필이 callback** 의 body 에 들어있는데,
-   **이게 최초 한번만** 옵니다.

홀리쉿

![img](../assets/signin-with-apple-sequence-diagram.png)


## callback 이 POST (..scope 을 지정했을때)

일반적인 OAuth provider 는 사용자 인증후에 token 교환에 필요한 `code` 와 위변조여부를 체크 하기 위한 `state` 를 줍니다.
근데 apple 은 여기에 사용자 profile 을 최초 한번만 줍니다.

    state=0b24bxxxxx&
    code=xxxxxxxxxxx.0.nsww.XX2b-XXXXKqm2JYHb4Pyeg&
    user=%7B%22name%22%3A%7B%22firstName%22%3A%22xxxxx%22%2C%22middleName%22%3A%22%22%2C%22lastName%22%3A%22xxxx%22%7D%2C%22email%22%3A%2227xxxxkb%40privaterelay.appleid.com%22%7D

이런식인데 body 에 담아서 POST request 로 주는데 그 와중에 `user` keyword 는 또 json string 입니다.
사용자가 최초로 인증한 이후에는 `user` parameter 없이 옵니다.

보통의 경우에는 `access_token` 으로 사용자 profile 을 fetch 할 수 있는 REST API 를 제공하는데,
apple 은 그런 api 없습니다.(못찾음)


## Create a signed jwt

appleid 에 OAuth application 설정할때에 `.p8` key 파일을 줍니다.
이 key 파일로 <https://appleid.apple.com/auth/token> 에 전달할 jwt 를 sign 합니다.
이 jwt 가 `client_secret` 입니다.

expiration 때문에 생성해놓지 못하고 요청마다 생성해서 전달해야 합니다.


# passport-apple

typescript 프로젝트에서 [passport](http://www.passportjs.org/) 를 사용했습니다.
<https://github.com/ananay/passport-apple> 이라는 훌륭한 오픈소스가 있지만 이걸로는 부족합니다.
scope 을 지원하지 않고, token 으로 profile 을 얻는 구조가 아니라서 hacky 하게 prototype 건들여가면서 고쳐써야 합니다.

여력이 있다면 남들도 잘 쓸 수 있게 고쳐서 PR 하고 싶은데 능력과 여력이 안됩니다.


# See Also

-   <https://sarunw.com/posts/sign-in-with-apple-1/>
-   <https://developer.okta.com/blog/2019/06/04/what-the-heck-is-sign-in-with-apple>
-   <https://forums.developer.apple.com/thread/118209>
