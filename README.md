# Learning react antive with spring boot
This project is for my personal learning purposes.
Learning sources and references:
- https://www.youtube.com/watch?v=h7QcSe-LYZg
- https://github.com/oktadev/okta-react-native-spring-boot-example

# React Native mobile app, Spring Boot API, and OIDC Authentication
 
A React Native and Spring Boot app with the following features:

* Secure, Spring Boot API
* React Native app that works on iOS or Android
* Production API on Cloud Foundry
* Production API on Google Cloud (via Kubernetes and GKE)
* OIDC Login with Okta or Keycloak

_All generated by JHipster and Ignite JHipster!_ 👏❤️

Please read [Build a Mobile App with React Native and Spring Boot](https://developer.okta.com/blog/2018/10/10/react-native-spring-boot-mobile-app) to see how this app was created.

**Prerequisites:** [Java 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) and [Node 8](https://nodejs.org).

> [Okta](https://developer.okta.com/) has standards-based APIs that support OIDC, OAuth 2.0, PKCE, and SAML. They're legit, you should [check them out](https://developer.okta.com/reference/).

* [Getting Started](#getting-started)
* [Links](#links)
* [Help](#help)
* [License](#license)

## Getting Started

To download this example locally, run the following commands:

```bash
git clone https://github.com/oktadeveloper/okta-react-native-spring-boot-example.git
cd okta-react-native-spring-boot-example
```

### Create a Web Application in Okta

You will need to create an OpenID Connect Application in Okta to get your values to perform authentication. 
Log in to your Okta Developer account and navigate to **Applications** > **Add Application**. Click **Web** and click **Next**. Give the app a name you'll remember, specify `http://localhost:8080/login` as a Login redirect URI, and click **Done**. Note the client ID and secret. You'll need to copy/paste them into a file in a minute.

Create a `ROLE_ADMIN` and `ROLE_USER` group (**Users** > **Groups** > **Add Group**) and add users to them. I recommend adding the account you signed up with to `ROLE_ADMIN` and creating a new user (**Users** > **Add Person**) to add to `ROLE_USER`.

Navigate to **API** > **Authorization Servers** and click the one named **default** to edit it. Click the **Claims** tab and **Add Claim**. Name it "groups", and include it in the ID Token. Set the value type to "Groups" and set the filter to be a Regex of `.*`. Click **Create** to complete the process.

Create a file on your hard drive called `~/.okta.env` and specify the settings for your app in it.

```
#!/bin/bash

# Okta with JHipster

export SECURITY_OAUTH2_CLIENT_ACCESS_TOKEN_URI="https://{yourOktaDomain}/oauth2/default/v1/token"
export SECURITY_OAUTH2_CLIENT_USER_AUTHORIZATION_URI="https://{yourOktaDomain}/oauth2/default/v1/authorize"
export SECURITY_OAUTH2_RESOURCE_USER_INFO_URI="https://{yourOktaDomain}/oauth2/default/v1/userinfo"
export SECURITY_OAUTH2_CLIENT_CLIENT_ID="{yourClientId}"
export SECURITY_OAUTH2_CLIENT_CLIENT_SECRET="{yourClientSecret}"
```

TIP: Make sure your URI variables do not have `-admin` in them. This is a common mistake.

### Create a Native Application in Okta

Ignite JHipster leverages [React Native AppAuth](https://github.com/FormidableLabs/react-native-app-auth), an SDK for communicating with OAuth 2.0 providers. It supports PKCE instead of a client secret, which is a more secure configuration. To use PKCE, you'll need to create a new Native application in Okta.

Log in to your Okta Developer account and navigate to **Applications** > **Add Application**. Click **Native** and click **Next**. Give the app a name you'll remember (e.g., `React Native`), select `Refresh Token` as a grant type, in addition to the default `Authorization Code`. Change the **Login redirect URI** to be `healthpoints://authorize` and click **Done**.

Modify `react-native-app/app/modules/login/login.sagas.js` to use the generated clientId.

```js
const { issuer, scope } = authInfo.data
const config = {
  issuer,
  clientId: '{yourNativeClientId}',
  scopes: scope.split(' '),
  redirectUrl: `${AppConfig.appUrlScheme}://authorize`
}
```

### Start Spring Boot API 

In the terminal where you want to run the Spring Boot API, run `source ~/.okta.env`, followed by `./gradlew`. You should be able to login using Okta at `http://localhost:8080`.

### Start React Native App

Open a new terminal and navigate to the `react-native-app` directory. Run `yarn` to install all the dependencies (`brew install yarn` if you don't have it). Then run `react-native run-ios` or `react-native run-android` to start an emulator with the app running in it. Want more info? Read the [blog post](https://developer.okta.com/blog/2018/10/10/react-native-spring-boot-mobile-app). ;)

## Links

This example uses some excellent open source projects:

* [Spring Boot](https://spring.io/projects/spring-boot)
* [React Native](https://facebook.github.io/react-native/)
* [Spring Security](https://spring.io/projects/spring-security)
* [React](https://reactjs.org/)
* [JHipster](https://www.jhipster.tech/)
* [Ignite JHipster](https://github.com/ruddell/ignite-jhipster)
* [Elasticsearch](https://www.elastic.co/products/elasticsearch)
* [TypeScript](https://www.typescriptlang.org/)
* [Java](https://openjdk.java.net/)

And some kick-ass platforms:

* [Cloud Foundry](https://www.cloudfoundry.org/)
* [Kubernetes](https://kubernetes.io/)

## Help

Please post any questions as comments on the [companion blog post](https://developer.okta.com/blog/2018/10/10/react-native-spring-boot-mobile-app), or visit our [Okta Developer Forums](https://devforum.okta.com/). You can also email developers@okta.com if you would like to create a support ticket.

## License

Apache 2.0, see [LICENSE](LICENSE).
