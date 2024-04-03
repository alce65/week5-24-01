# Test del servicio con HttpClient

Para los test del servicio que encapsula httpClient, Angular proporciona una serie de herramientas
que importaremos desde **'@angular/common/http/testing'**

- HttpClientTestingModule
- HttpTestingController
- TestRequest

El módulo HttpClientTestingModule se importa en el test para que funcione como provider

```ts
    TestBed.configureTestingModule({
      imports: [HttpClientTestingModule],
    });
```

El controller es la instancia de HttpTestingController que hará de mock de fetch para crear la request http

```ts
 let controller: HttpTestingController;
 ...
 controller = TestBed.inject(HttpTestingController)
```

Además definiremos una constante con la url que esperaríamos que usara nuestro servicio.

```ts
  const expectedUrl = new URL('courses', environment.baseUrl).href;
```

En el test de un método que use httpClient

- nos suscribimos al método, incluyendo los asserts en la suscripción

- creamos la request http simulada

```ts
  const testRequest: TestRequest = controller.expectOne(expectedUrl);
  expect(testRequest.request.method).toEqual('GET');
```

- lanzamos la request definiendo la response moqueada que queremos que le llegue a la suscripción

```ts
  testRequest.flush([]);
```

Por ejemplo, para el método getAll

```ts
  service.getAll().subscribe((courses) => {
    expect(courses).toEqual([]);
  });

  const testRequest: TestRequest = controller.expectOne(expectedUrl);
  expect(testRequest.request.method).toEqual('GET');

  testRequest.flush([]);
```

En el caso del método add, además de los asserts de la suscripción, habrá que comprobar que la request lleva el body que esperamos

```ts
  service.add({ id: '1' }).subscribe(([course]) => {
    expect(course).toEqual({ id: '1' } as Course);
  });

  const testRequest: TestRequest = controller.expectOne(expectedUrl);
  expect(testRequest.request.method).toEqual('POST');
  expect(testRequest.request.body).toEqual({ id: '1' });

  testRequest.flush({ id: '1' });
```

En el caso del método update, además de los asserts de la suscripción, habrá que comprobar que la request lleva el body que esperamos y la url que esperamos

```ts
  service.update('1', { id: '1' }).subscribe(([course]) => {
    expect(course).toEqual({ id: '1' } as Course);
  });

  const testRequest: TestRequest = controller.expectOne(expectedUrl + '/1');
  expect(testRequest.request.method).toEqual('PATCH');
  expect(testRequest.request.body).toEqual({ id: '1' });

  testRequest.flush({ id: '1' });
```

En el caso del método delete, además de los asserts de la suscripción, habrá que comprobar que la request lleva la url que esperamos

```ts
  service.delete('1').subscribe(([course]) => {
    expect(course).toEqual({ id: '1' } as Course);
  });

  const testRequest: TestRequest = controller.expectOne(expectedUrl + '/1');
  expect(testRequest.request.method).toEqual('DELETE');

  testRequest.flush({ id: '1' });
```
