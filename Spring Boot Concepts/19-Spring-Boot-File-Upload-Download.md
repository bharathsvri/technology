# Spring Boot File Upload and Download

## File Operations in Spring Boot

Spring Boot makes it easy to handle file uploads and downloads in web applications using Spring MVC's multipart support.

## File Upload Configuration

### Multipart Configuration

```properties
# File Upload Configuration
spring.servlet.multipart.enabled=true
spring.servlet.multipart.max-file-size=10MB
spring.servlet.multipart.max-request-size=10MB
spring.servlet.multipart.file-size-threshold=2KB
```

## Single File Upload

### Controller

```java
@RestController
@RequestMapping("/api/files")
public class FileController {
    
    private final FileStorageService fileStorageService;
    
    @PostMapping("/upload")
    public ResponseEntity<FileUploadResponse> uploadFile(
            @RequestParam("file") MultipartFile file) {
        
        if (file.isEmpty()) {
            return ResponseEntity.badRequest()
                .body(new FileUploadResponse("File is empty", null));
        }
        
        String fileName = fileStorageService.storeFile(file);
        String fileDownloadUri = ServletUriComponentsBuilder
            .fromCurrentContextPath()
            .path("/api/files/download/")
            .path(fileName)
            .toUriString();
        
        return ResponseEntity.ok(new FileUploadResponse(
            "File uploaded successfully", fileDownloadUri));
    }
}
```

### Service

```java
@Service
public class FileStorageService {
    
    private final Path fileStorageLocation;
    
    public FileStorageService(@Value("${file.upload-dir}") String uploadDir) {
        this.fileStorageLocation = Paths.get(uploadDir).toAbsolutePath().normalize();
        
        try {
            Files.createDirectories(this.fileStorageLocation);
        } catch (Exception ex) {
            throw new FileStorageException("Could not create directory", ex);
        }
    }
    
    public String storeFile(MultipartFile file) {
        String originalFileName = StringUtils.cleanPath(file.getOriginalFilename());
        
        try {
            if (originalFileName.contains("..")) {
                throw new FileStorageException("Invalid file path");
            }
            
            String fileName = UUID.randomUUID().toString() + "_" + originalFileName;
            Path targetLocation = this.fileStorageLocation.resolve(fileName);
            Files.copy(file.getInputStream(), targetLocation, 
                      StandardCopyOption.REPLACE_EXISTING);
            
            return fileName;
        } catch (IOException ex) {
            throw new FileStorageException("Could not store file", ex);
        }
    }
    
    public Resource loadFileAsResource(String fileName) {
        try {
            Path filePath = this.fileStorageLocation.resolve(fileName).normalize();
            Resource resource = new UrlResource(filePath.toUri());
            
            if (resource.exists()) {
                return resource;
            } else {
                throw new FileNotFoundException("File not found: " + fileName);
            }
        } catch (MalformedURLException ex) {
            throw new FileNotFoundException("File not found: " + fileName);
        }
    }
}
```

## Multiple File Upload

### Controller

```java
@PostMapping("/upload-multiple")
public ResponseEntity<List<FileUploadResponse>> uploadMultipleFiles(
        @RequestParam("files") MultipartFile[] files) {
    
    List<FileUploadResponse> responses = new ArrayList<>();
    
    for (MultipartFile file : files) {
        if (!file.isEmpty()) {
            String fileName = fileStorageService.storeFile(file);
            String fileDownloadUri = ServletUriComponentsBuilder
                .fromCurrentContextPath()
                .path("/api/files/download/")
                .path(fileName)
                .toUriString();
            
            responses.add(new FileUploadResponse(
                "File uploaded: " + file.getOriginalFilename(), fileDownloadUri));
        }
    }
    
    return ResponseEntity.ok(responses);
}
```

## File Download

### Download Controller

```java
@GetMapping("/download/{fileName:.+}")
public ResponseEntity<Resource> downloadFile(@PathVariable String fileName) {
    Resource resource = fileStorageService.loadFileAsResource(fileName);
    
    String contentType = null;
    try {
        contentType = request.getServletContext()
            .getMimeType(resource.getFile().getAbsolutePath());
    } catch (IOException ex) {
        // Could not determine file type
    }
    
    if (contentType == null) {
        contentType = "application/octet-stream";
    }
    
    return ResponseEntity.ok()
        .contentType(MediaType.parseMediaType(contentType))
        .header(HttpHeaders.CONTENT_DISPOSITION, 
                "attachment; filename=\"" + resource.getFilename() + "\"")
        .body(resource);
}
```

## File Validation

### Validating File Type

```java
@Service
public class FileValidationService {
    
    private static final List<String> ALLOWED_EXTENSIONS = 
        Arrays.asList("jpg", "jpeg", "png", "pdf", "doc", "docx");
    
    public boolean isValidFile(MultipartFile file) {
        String fileName = file.getOriginalFilename();
        if (fileName == null) {
            return false;
        }
        
        String extension = fileName.substring(fileName.lastIndexOf(".") + 1)
            .toLowerCase();
        
        return ALLOWED_EXTENSIONS.contains(extension);
    }
    
    public boolean isValidFileSize(MultipartFile file, long maxSize) {
        return file.getSize() <= maxSize;
    }
}
```

### Using Validation in Controller

```java
@PostMapping("/upload")
public ResponseEntity<FileUploadResponse> uploadFile(
        @RequestParam("file") MultipartFile file) {
    
    if (!fileValidationService.isValidFile(file)) {
        return ResponseEntity.badRequest()
            .body(new FileUploadResponse("Invalid file type", null));
    }
    
    if (!fileValidationService.isValidFileSize(file, 10 * 1024 * 1024)) {
        return ResponseEntity.badRequest()
            .body(new FileUploadResponse("File size exceeds limit", null));
    }
    
    // Process file upload
}
```

## File Storage Options

### Local File System

```java
@Value("${file.upload-dir}")
private String uploadDir;

private Path getFileStorageLocation() {
    return Paths.get(uploadDir).toAbsolutePath().normalize();
}
```

### Cloud Storage (AWS S3)

#### Add Dependency

```xml
<dependency>
    <groupId>com.amazonaws</groupId>
    <artifactId>aws-java-sdk-s3</artifactId>
</dependency>
```

#### S3 Service

```java
@Service
public class S3FileStorageService {
    
    private final AmazonS3 s3Client;
    
    @Value("${aws.s3.bucket}")
    private String bucketName;
    
    public String uploadFile(MultipartFile file) {
        String fileName = UUID.randomUUID().toString() + "_" + file.getOriginalFilename();
        
        try {
            ObjectMetadata metadata = new ObjectMetadata();
            metadata.setContentLength(file.getSize());
            metadata.setContentType(file.getContentType());
            
            s3Client.putObject(bucketName, fileName, file.getInputStream(), metadata);
            return fileName;
        } catch (IOException e) {
            throw new FileStorageException("Failed to upload file", e);
        }
    }
    
    public Resource downloadFile(String fileName) {
        S3Object s3Object = s3Client.getObject(bucketName, fileName);
        return new InputStreamResource(s3Object.getObjectContent());
    }
}
```

## Complete Example

### File Upload Response

```java
public class FileUploadResponse {
    private String message;
    private String fileDownloadUri;
    
    // Constructors, getters, setters
}
```

### Complete Controller

```java
@RestController
@RequestMapping("/api/files")
public class FileController {
    
    private final FileStorageService fileStorageService;
    private final FileValidationService fileValidationService;
    
    @PostMapping("/upload")
    public ResponseEntity<FileUploadResponse> uploadFile(
            @RequestParam("file") MultipartFile file) {
        
        if (file.isEmpty()) {
            return ResponseEntity.badRequest()
                .body(new FileUploadResponse("File is empty", null));
        }
        
        if (!fileValidationService.isValidFile(file)) {
            return ResponseEntity.badRequest()
                .body(new FileUploadResponse("Invalid file type", null));
        }
        
        String fileName = fileStorageService.storeFile(file);
        String fileDownloadUri = ServletUriComponentsBuilder
            .fromCurrentContextPath()
            .path("/api/files/download/")
            .path(fileName)
            .toUriString();
        
        return ResponseEntity.ok(new FileUploadResponse(
            "File uploaded successfully", fileDownloadUri));
    }
    
    @GetMapping("/download/{fileName:.+}")
    public ResponseEntity<Resource> downloadFile(@PathVariable String fileName) {
        Resource resource = fileStorageService.loadFileAsResource(fileName);
        
        return ResponseEntity.ok()
            .header(HttpHeaders.CONTENT_DISPOSITION, 
                    "attachment; filename=\"" + resource.getFilename() + "\"")
            .body(resource);
    }
}
```

## Best Practices

1. **Validate Files**: Always validate file type and size
2. **Sanitize Filenames**: Clean filenames to prevent path traversal
3. **Use Unique Filenames**: Use UUIDs to prevent conflicts
4. **Handle Exceptions**: Properly handle file operation exceptions
5. **Set Size Limits**: Configure appropriate size limits
6. **Use Cloud Storage**: Consider cloud storage for production
7. **Secure File Access**: Implement proper access control
8. **Clean Up Old Files**: Implement cleanup for old files

## Summary

Spring Boot File Operations:
- Easy to handle file uploads
- Supports single and multiple file uploads
- Enables file downloads
- Supports file validation
- Can use local or cloud storage
- Essential for file management features

