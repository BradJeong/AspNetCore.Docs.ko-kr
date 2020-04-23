<!-- THIS INCLUDE USED BY MVC AND RP -->
`Movie` 클래스에 다음 속성을 추가합니다.

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/Movie.cs?name=snippet1)]

`Movie` 클래스에는 다음이 포함됩니다.

* `ID` 필드는 데이터베이스에서 기본 키 대신 필요합니다.
* `[DataType(DataType.Date)]`:  [DataType](xref:System.ComponentModel.DataAnnotations.DataTypeAttribute) 특성은 데이터 형식(Date)을 지정합니다. 이 특성 사용:

  * 사용자가 날짜 필드에 시간 정보를 입력할 필요가 없습니다.
  * 시간 정보 없이 날짜만 표시됩니다.

[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations)는 이후 자습서에서 다룹니다.
