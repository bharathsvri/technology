# Testing HTTP with `HttpClientTestingController`

**`HttpTestingController`** verifies **outgoing** requests and **flushes** mock responses—preferred over mocking **`HttpClient`** ad hoc in Angular unit tests.

---

## Setup

Provide **`HttpClientTestingModule`** and inject **`HttpTestingController`** alongside the service under test.

---

## Typical flow

1. Call method that triggers **`http.get/post`**.
2. **`expectOne('/api/...')`** (or **`match`**) assert **method**, **url**, **headers**.
3. **`req.flush(body)`** or **`error(...)`** to simulate outcomes.
4. **`httpMock.verify()`** in **`afterEach`** to ensure **no outstanding** requests.

---

## Pitfalls

- **Multiple parallel calls** → use **`match`** and consume all, or **`expectOne`** fails.
- Forgetting **`verify()`** hides **unexpected** extra requests.

---

## Related

- [HttpClient and Interceptors](12-HttpClient-and-Interceptors.md)
- [Testing with Jasmine and Karma](24-Testing-Jasmine-Karma.md)
