ASP.NET Core ID는 ASP.NET Core 웹앱에 UI(사용자 인터페이스) 로그인 기능을 추가합니다. 웹 API 및 SPA를 보호하려면 다음 중 하나를 사용합니다.

* [Azure Active Directory](/azure/api-management/api-management-howto-protect-backend-with-aad)
* [Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-custom-rest-api-netfw)(Azure AD B2C)
* [IdentityServer4](https://identityserver.io)

IdentityServer4는 ASP.NET Core 3.0용 OpenID Connect 및 OAuth 2.0 프레임워크입니다. IdentityServer4에서는 다음과 같은 보안 기능을 사용할 수 있습니다.

* AaaS(Authentication as a Service)
* 여러 애플리케이션 유형에 대한 SSO(Single Sign-On/Off)
* API에 대한 액세스 제어
* 페더레이션 게이트웨이

자세한 내용은 [IdentityServer4 시작](http://docs.identityserver.io/en/latest/index.html)을 참조하세요.