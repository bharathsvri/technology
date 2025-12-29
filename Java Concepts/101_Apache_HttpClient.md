# Apache HttpClient

## What is Apache HttpClient?
Apache HttpClient is a library for making HTTP requests from Java applications.

## Basic GET Request
```java
import org.apache.http.client.methods.*;
import org.apache.http.impl.client.*;
import org.apache.http.util.EntityUtils;

CloseableHttpClient httpClient = HttpClients.createDefault();
HttpGet request = new HttpGet("https://api.example.com/data");

try (CloseableHttpResponse response = httpClient.execute(request)) {
    HttpEntity entity = response.getEntity();
    String result = EntityUtils.toString(entity);
    System.out.println(result);
}
```

## POST Request
```java
HttpPost request = new HttpPost("https://api.example.com/data");
request.setHeader("Content-Type", "application/json");

String json = "{\"name\":\"John\",\"age\":25}";
request.setEntity(new StringEntity(json, ContentType.APPLICATION_JSON));

try (CloseableHttpResponse response = httpClient.execute(request)) {
    int statusCode = response.getStatusLine().getStatusCode();
    String result = EntityUtils.toString(response.getEntity());
}
```

## Request Configuration
```java
RequestConfig config = RequestConfig.custom()
    .setConnectTimeout(5000)
    .setSocketTimeout(10000)
    .setConnectionRequestTimeout(5000)
    .build();

HttpGet request = new HttpGet("https://api.example.com/data");
request.setConfig(config);
```

## Authentication
```java
CredentialsProvider credentialsProvider = new BasicCredentialsProvider();
credentialsProvider.setCredentials(
    AuthScope.ANY,
    new UsernamePasswordCredentials("username", "password"));

CloseableHttpClient httpClient = HttpClients.custom()
    .setDefaultCredentialsProvider(credentialsProvider)
    .build();
```

## Best Practices
1. Use connection pooling
2. Set appropriate timeouts
3. Handle exceptions properly
4. Close resources
5. Use try-with-resources
6. Handle redirects
7. Implement retry logic
8. Monitor connection usage

