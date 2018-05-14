# <a name="how-to-buildrun-secure-user-data-sample"></a>보안 사용자 데이터 샘플을 빌드/실행 하는 방법

* 암호 관리자 도구와 함께 암호를 설정 합니다.

  `dotnet user-secrets set SeedUserPW <pw>`

* 데이터베이스를 업데이트 합니다.

    `dotnet ef database update`
