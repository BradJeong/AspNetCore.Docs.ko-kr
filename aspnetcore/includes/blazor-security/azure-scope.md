> [!NOTE]
> Azure Portal에서 앱에 대 한 범위 URI를 제공 하 고 앱이 API에서 *401 권한 없음* 응답을 받을 때 처리 되지 않은 예외를 throw 하는 경우 스키마와 호스트를 포함 하지 않는 범위 uri를 사용해 보세요. 예를 들어 Azure Portal는 다음 범위 URI 형식 중 하나를 제공할 수 있습니다.
>
> * `https://{TENANT}.onmicrosoft.com/{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
> * `api://{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
>
> 스키마 및 호스트 없이 범위 URI를 제공 해 보세요.
>
> ```csharp
> options.ProviderOptions.DefaultAccessTokenScopes.Add(
>     "{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```
