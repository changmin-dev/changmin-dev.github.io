---
layout: post
title: Yeoman으로 생성한 프로젝트에서 .NetCore 2.0 수동적용 
---
Yeoman으로 프로젝트를 생성하면 1번대 버전으로 프로젝트가 생성된다. 그리고 1번대 버전 이 제대로 설치 되지 않으면 `dotnet restore`명령을 사용하면 에러가 발생한다. 나는 [튜토리얼](https://docs.microsoft.com/ko-kr/dotnet/core/docker/building-net-docker-images#creating-the-web-api-application)을 하면서 문제가 발생했다.

## 해결하기
몇가지 파일에 값을 바꾸어주면 정상적으로 작동한다.

1. global.json파일  
version을 2.0.0으로 바꾼다.
```
{
  "sdk": {
    "version": "2.0.0"
  }
}
```

2. WeatherMicroservice.csproj  
netcoreapp2.0으로 바꾸어 준다. Version="2.0.0"으로 바꾸어 준다.
```
<Project ToolsVersion="15.0" Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <Folder Include="wwwroot\" />
  </ItemGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="2.0.0" />
  </ItemGroup>

</Project>
```

## 실행
이후 `dotnet restore`명령어로 정상적으로 진행이 가능하다.