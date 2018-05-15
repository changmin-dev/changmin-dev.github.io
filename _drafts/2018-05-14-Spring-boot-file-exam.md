---
layout: post
title : Spring boot의 파일 업로드와 다운로드 예제 분석하기
---

이번에 진행할 프로젝트가 이미지 파일을 다루는 것이다. 하지만 Spring을 잘하는 편도 아니고 html과 json말고는 파일 응답을 준적이 없어서 파일을 다루는 튜토리얼을 찾아보았다.
스프링 문서에서 [관련 예제](https://spring.io/guides/gs/uploading-files/)를 제공한다. 페이지 이동형이다. 템플릿엔진인 타임리프도 사용해야 되고 적절하지 않다고 판단했다. [Rest 형태](https://www.callicoder.com/spring-boot-file-upload-download-rest-api-example/)의 예제를 찾아서 이것을 분석하려고 한다.

## 프로젝트 개요
[분석할 프로젝트](https://github.com/callicoder/spring-boot-file-upload-download-rest-api-example)는 Rest 스타일의 API를 통해서 파일을 업로드하고 다운로드하는 프로젝트이다.

## FileStorageProperties
프로퍼티를 설정하는 부분이다. `application.properties`에서 `file.upload-dir`의 값을 가져와서 사용하게 해준다. 이 프로퍼티는 업로드될 경로를 지정한다. 프로퍼티 파일에서는 네이밍이 Kebab 케이스(-으로 단어 구분)이지만, 자바 파일에서는 Camel케이스이다.
```java
@ConfigurationProperties(prefix = "file")
public class FileStorageProperties {
    private String uploadDir;

    public String getUploadDir() {
        return uploadDir;
    }

    public void setUploadDir(String uploadDir) {
        this.uploadDir = uploadDir;
    }
}
```

이 프로퍼티는 커스텀 프로퍼티이므로 다음과 같이 등록해준다. 
```java
@SpringBootApplication
@EnableConfigurationProperties({FileStorageProperties.class})
public class FileApplication {

	public static void main(String[] args) {
		SpringApplication.run(FileApplication.class, args);
	}
}
```

## Exception 처리 클래스
요청한 파일이 없는 경우 발생하는 예외이다. 별도로 `@ResponseStatus`를 이용해서 HTTP 상태 응답을 설정하는 것 인상 깊었다. 프로젝트를 실행하면 파일이 없는 경우 Exception을 던지는데 이때 응답이 404가 나온다.  
저장에서 문제가 있는 경우 `FileStorageException`이 있는데 거의 같은 내용으로 되어 있다. 
```java
@ResponseStatus(HttpStatus.NOT_FOUND)
public class RequestFileNotFoundException extends RuntimeException {
    public RequestFileNotFoundException(String message) {
        super(message);
    }

    public RequestFileNotFoundException(String message, Throwable cause) {
        super(message, cause);
    }
}
```

## DTO(Data Transfer Object)
 DTO는 전달하려는 내용을 정의하는데, 원래 데이터에서 전달하고 싶은 부분만 정의해서 사용할 수 있다. 
이 클래스는 업로드 되는 파일의 정보를 나타낸다.
```java
public class UploadFileResponse {
    private String fileName;
    private String fileDownloadUri;
    private String fileType;
    private long size;
...
//getter setter 생성자
```

## FileStorageService
`@Service`는 비즈니스 로직에 사용된다. 이 어노테이션을 통해서 DI 컨테이너가 생성하고 주입할 수 있도록 식별된다. 

### 생성자
FileStorageService의 내용을 보면 생성자로 위에서 정의한 프로퍼티에서 Path를 가지고 온다. 이를 통해서 경로를 만든다. `java.nio.file`패키지의 Path, Files, Paths가 사용된다. 
```java
//Path는 파일의 위치를 나타내는데 사용되는 인터페이스
private final Path fileStorageLocation;

@Autowired
public FileStorageService(FileStorageProperties fileStorageProperties) {
    
    this.fileStorageLocation = Paths.get(fileStorageProperties.getUploadDir())
            .toAbsolutePath()
            .normalize();//중복되는것 제거하기

    try {
        Files.createDirectories(this.fileStorageLocation);
    } catch (IOException e) {
        throw new FileStorageException("Could not create the directory ");
    }
}
```

### 파일 저장하기
파일 업로드에 사용되는 클래스는 MultipartFile이다. mime의 Multipart가 생각나는 이름이다. `getInputStream()` 메서드를 통해서 스트림 접근을 한다. 이 스트림을 target에 저장한다.
```java
public String storeFile(MultipartFile file) {
    String fileName = StringUtils.cleanPath(file.getOriginalFilename());

    try {
        // 보안적인 이유로 상위 폴더 경로는 접근하지 못하게한다.
        if(fileName.contains("..")) {
            throw new FileStorageException("Sorry! Filename contains invalid path sequence" + fileName);
        }

        Path targetLocation = this.fileStorageLocation.resolve(fileName);
        Files.copy(file.getInputStream(), targetLocation, StandardCopyOption.REPLACE_EXISTING);
        return fileName;
    } catch (IOException ex) {
        throw new FileStorageException("Could not store file " + fileName + ". Please try again!", ex);
    }
}
```

### 파일 불러오기
파일 불러오기는 resource를 사용한다. 파일을 찾지 못하는 경우 Exception 정의에 따라 404 Error가 발생한다.
```java
public Resource loadFileAsResource(String fileName) {
    try {
        Path filePath = this.fileStorageLocation.resolve(fileName).normalize();
        Resource resource = new UrlResource(filePath.toUri());

        if(resource.exists()) {
            return resource;
        } else {
            throw new RequestFileNotFoundException("File not found " + fileName);
        }
    } catch (MalformedURLException e) {
        throw new RequestFileNotFoundException("File not found " + fileName, e);
    }
}
```

## FileController
Handler클래스이다. 요청에 따른 결과를 돌려준다. `@RestController`는 응답으로 주어지는 `@ResponseBody`를 생략할 수 있다.
```java
@RestController
public class FileController {
    ...
}
```

### 생성자를 통한 의존성 주입
Service 객체를 DI 컨테이너를 통해서 생성하고 DI 컨테이너에서 연결한다. 
```java
private FileStorageService fileStorageService;

@Autowired
public FileController(FileStorageService fileStorageService){
    this.fileStorageService = fileStorageService;
}
```
### 업로드
`MultipartFile`로 파일을 다룬다. 파일을 성공적으로 저장하면 파일의 정보를 리턴한다. 
```java
 @PostMapping("/uploadFile")
public UploadFileResponse uploadFile(@RequestParam("file") MultipartFile file){
    String fileName = fileStorageService.storeFile(file);
    String fileDownloadUri = ServletUriComponentsBuilder.fromCurrentContextPath()
            .path("/downloadFile")
            .path(fileName)
            .toUriString();

    return new UploadFileResponse(fileName, fileDownloadUri, file.getContentType(), file.getSize());
}
```
다중 파일도 지원한다. 배열로 받으므로 출력은 List로 바꾸어야 한다. List가 JSON형태의 배열로 바뀌어 리턴한다. Java8 문법이 사용 되었다. 하나씩 확인해 보면 stream을 통해서 추상화하고 map으로 이전에 만든 단일 업로드 함수를 요소들에 적용한다. 다시 collect를 이용해 List로 만들어 온다. 
```java
@PostMapping("/uploadMultipleFiles")
public List<UploadFileResponse> uploadMultipleFiles(@RequestParam("files") MultipartFile[] files){
    return Arrays.asList(files)
            .stream()
            .map(file -> uploadFile(file))
            .collect(Collectors.toList());
}
```
### 다운로드
`application/octet-stream`은 파일 타입을 알 수 없을때 사용한다. 나머지는 MIME을 확인해서 타입을 알아낸다.
응답은 ResponseEntity을 이용해서 HTTP응답의 요소들을 지정해서 리턴한다. [Content-Disposition](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Disposition)은 보여질 것으로 예상되는 정보를 알려주는 용도로 사용된다.
```java
@GetMapping("/downloadFile/{fileName:.+}")
public ResponseEntity<Resource> downloadFile(@PathVariable String fileName, HttpServletRequest request) {
    Resource resource = fileStorageService.loadFileAsResource(fileName);

    String contentType = null;
    try {
        contentType = request.getServletContext().getMimeType(resource.getFile().getAbsolutePath());
    }catch (IOException ex){
        logger.info("Could not determine file type");
    }

    if(contentType == null){
        contentType = "application/octet-stream";
    }

    return ResponseEntity.ok()
            .contentType(MediaType.parseMediaType(contentType))
            .header(HttpHeaders.CONTENT_DISPOSITION,
                    "attachment: filename=\"" + resource.getFilename() + "\"")
            .body(resource);
}
```

# 결론
Spring-boot 프로젝트를 전체적으로 다시 볼 수 있었다. 과거 프로젝트에서는 JPA를 이용해서 CRUD만 하므로 따로 Service를 만들어 주지 않았는데 Service가 어떻게 쓰이는지 확인할 수 있었다. 또한 커스텀 프로퍼티 설정도 확인할 수 있었다. Spring에서 파일을 다루는 데는 `org.springframework.core.io`의 Resource, `java.nio.file`패키지의 요소들 그리고 `MultipartFile`등 이 사용되는 것을 확인했다.  시간이 될지 모르지만 git flow를 적용해서 과거 flask 프로젝트의 Spring버전을 만들어 봐서 확실히 숙달해야겠다.