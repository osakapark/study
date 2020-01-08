# 19. MVC5에서 Unity Container를 이용한 Dependency Injection
## 1. 가상의 솔루션 폴더 생성
BusinessLogicLayer
DataAccessLayer
Common
PresentationLayer

## 2. Model Class 생성
- Hi.Model
<br> 새 프로젝트 추가 > Visual C# > Windows Desktop > 클래스 라이브러리(.NET Framework)

## 3. DataAccessLayer
- HI.IDAL
새 프로젝트 추가 > 설치됨 > 클래스 라이브러리(.NET Framework)

## 4. PresentationLayer
- Hi.MVC5
새 프로젝트 추가 > 설치됨 > ASP.NET 웹 응용 프로그램(.NET Framework) > MVC(템플릿 선택)
- Unity.Mvc 추가 (Open Source Project) 
    - NuGet
- UnitConfig.cs    
```c#
public static void RegisterTypes(IUnityContainer container)
{
    // NOTE: To load from web.config uncomment the line below.
    // Make sure to add a Unity.Configuration to the using statements.
    // container.LoadConfiguration();

    // TODO: Register your type's mappings here.
    // container.RegisterType<IProductRepository, ProductRepository>();

    container.RegisterType<UserBll>();
    container.RegisterType<IUserDal, UserDal>();
}
```
