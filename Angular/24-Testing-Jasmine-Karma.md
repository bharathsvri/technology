# Testing with Jasmine and Karma / Jest

## `TestBed`

Configure standalone component testing:

```ts
await TestBed.configureTestingModule({
  imports: [UserComponent],
}).compileComponents();
```

## `fixture.detectChanges`

Trigger change detection in tests.

## DOM queries

`fixture.nativeElement.querySelector`, `debugElement`.

## HTTP testing

`HttpClientTestingModule`, `HttpTestingController` to mock backend.

Next: [i18n](25-Internationalization-i18n.md).
