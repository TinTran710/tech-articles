
[Source](http://docs.guzzlephp.org/en/stable/quickstart.html "Permalink to Quickstart — Guzzle Documentation")

# Quickstart — Guzzle Docs

Bài viết này sẽ giới thiệu nhanh về Guzzle và cung cấp một số ví dụ mang tính giới thiệu. Nếu bạn chưa cài Guzzle, hãy tới trang [Cài đặt][1].

## Tạo Request

Bạn có thể gửi các request với Guzzle bằng cách sử dụng đối tượng `GuzzleHttpClientInterface`.

### Tạo Client
    
    
    use GuzzleHttpClient;
    
    $client = new Client([
        // Base URI is used with relative requests
        'base_uri' => 'http://httpbin.org',
        // You can set any number of default request options.
        'timeout'  => 2.0,
    ]);
    

Các client là không đổi trong Guzzle 6, điều đó có nghĩa rằng bạn không thể thay đổi các tùy chọn mặc định được sử dụng bởi client sau khi nó được khởi tạo.

Constructor của client có thể nhận các tùy chọn dưới dạng một mảng: 

`base_uri`
: 

(string|UriInterface) URI gốc của client được nối với các URI tương đối. Đó có thể là một string hoặc là một thực thể UriInterface. Khi client có một URI tương đối, client sẽ kết hợp URI gốc với URI tương đối dựa theo luật được mô tả trong [RFC 3986, section 2][2].
    
    
    // Create a client with a base URI
    $client = new GuzzleHttpClient(['base_uri' => 'https://foo.com/api/']);
    // Send a request to https://foo.com/api/test
    $response = $client->request('GET', 'test');
    // Send a request to https://foo.com/root
    $response = $client->request('GET', '/root');
    

Cảm thấy không muốn đọc RFC 3986? Vậy thì đây là một vài ví dụ nhanh về cách mà một `base_uri` được xử lý với URI khác.

| base_uri              | URI              | Result                   |  
| --------------------- | ---------------- | ------------------------ |  
| `http://foo.com`      | `/bar`           | `http://foo.com/bar`     |  
| `http://foo.com/foo`  | `/bar`           | `http://foo.com/bar`     |  
| `http://foo.com/foo`  | `bar`            | `http://foo.com/bar`     |  
| `http://foo.com/foo/` | `bar`            | `http://foo.com/foo/bar` |  
| `http://foo.com`      | `http://baz.com` | `http://baz.com`         |  
| `http://foo.com/?bar` | `bar`            | `http://foo.com/bar`     |  

`handler`
: (callable) Hàm transfer các HTTP request trên đường dẫn. Hàm được gọi với một 
`Psr7HttpMessageRequestInterface` và mảng các tùy chọn transfer, và phải trả về một `GuzzleHttpPromisePromiseInterface` mà được kèm với `Psr7HttpMessageResponseInterface` nếu thành công. `handler` là một tùy chọn duy nhất của constructor mà không thể  bị ghi đề trên mỗi tùy chọn request

`...`
: (mixed) Tất cả các tùy chọn khác được truyền vào hàm constructor được sử dụng như các tùy chọn request mặc định với mỗi request được tạo bởi client.


### Gửi Requests

Các Magic method trên client khiến việc gửi các request đồng bộ trở nên dễ dàng:
    
    
    $response = $client->get('http://httpbin.org/get');
    $response = $client->delete('http://httpbin.org/delete');
    $response = $client->head('http://httpbin.org/get');
    $response = $client->options('http://httpbin.org/get');
    $response = $client->patch('http://httpbin.org/patch');
    $response = $client->post('http://httpbin.org/post');
    $response = $client->put('http://httpbin.org/put');
    

Bạn có thể tạo một request và sau đó dùng client để gửi request khi bạn sẵn sàng:
    
    
    use GuzzleHttpPsr7Request;
    
    $request = new Request('PUT', 'http://httpbin.org/put');
    $response = $client->send($request, ['timeout' => 2]);
    

Các đối tượng client mang lại một giải pháp linh động trong cách vận chuyển các request bao gồm các tùy chọn request mặc định, các stack middleware handler mặc định được sử dụng bởi mỗi request, và một URI gốc cho phép bạn gửi các request với các URI tương đối.


Bạn có thể tìm hiểu thêm về client middleware tại [_Handlers and Middleware_][3] trong tài liệu.

### Async Requests (Request bất đồng bộ)

Bạn có thể gửi các request bất đồng bộ bằng cách sử dụng các magic method được cung cấp bởi client:
    
    
    $promise = $client->getAsync('http://httpbin.org/get');
    $promise = $client->deleteAsync('http://httpbin.org/delete');
    $promise = $client->headAsync('http://httpbin.org/get');
    $promise = $client->optionsAsync('http://httpbin.org/get');
    $promise = $client->patchAsync('http://httpbin.org/patch');
    $promise = $client->postAsync('http://httpbin.org/post');
    $promise = $client->putAsync('http://httpbin.org/put');
    

Bạn cũng có thể sử dụng các hàm sendAsync() và requestAsync() của client:
    
    
    use GuzzleHttpPsr7Request;
    
    // Create a PSR-7 request object to send
    $headers = ['X-Foo' => 'Bar'];
    $body = 'Hello!';
    $request = new Request('HEAD', 'http://httpbin.org/head', $headers, $body);
    
    // Or, if you don't need to pass in a request instance:
    $promise = $client->requestAsync('GET', 'http://httpbin.org/get');
    

Các promise được trả về bởi những hàm trên có cài đặt, tuân theo [Promises/A+ spec][4], được cung cấp bởi [Guzzle promises library][5]. Điều này có nghĩa rằng bạn có thể gọi nối hàm `then()` sau promise. Những lời gọi phía sau này hoặc có kèm một `PsrHttpMessageResponseInterface` thành công hoặc bị từ chối với một ngoại lệ.
    
    
    use PsrHttpMessageResponseInterface;
    use GuzzleHttpExceptionRequestException;
    
    $promise = $client->requestAsync('GET', 'http://httpbin.org/get');
    $promise->then(
        function (ResponseInterface $res) {
            echo $res->getStatusCode() . "n";
        },
        function (RequestException $e) {
            echo $e->getMessage() . "n";
            echo $e->getRequest()->getMethod();
        }
    );
    

### Các request đồng thời

Bạn có thể gửi nhiều request đồng thời sử dụng các promise và các request bất đồng bộ.
    
    
    use GuzzleHttpClient;
    use GuzzleHttpPromise;
    
    $client = new Client(['base_uri' => 'http://httpbin.org/']);
    
    // Initiate each request but do not block
    $promises = [
        'image' => $client->getAsync('/image'),
        'png'   => $client->getAsync('/image/png'),
        'jpeg'  => $client->getAsync('/image/jpeg'),
        'webp'  => $client->getAsync('/image/webp')
    ];
    
    // Wait on all of the requests to complete. Throws a ConnectException
    // if any of the requests fail
    $results = Promiseunwrap($promises);
    
    // Wait for the requests to complete, even if some of them fail
    $results = Promisesettle($promises)->wait();
    
    // You can access each result using the key provided to the unwrap
    // function.
    echo $results['image']['value']->getHeader('Content-Length')[0]
    echo $results['png']['value']->getHeader('Content-Length')[0]
    

Bạn có thể sử dụng đối tượng `GuzzleHttpPool` khi bạn không xác định được số lượng request cần gửi.
    
    
    use GuzzleHttpPool;
    use GuzzleHttpClient;
    use GuzzleHttpPsr7Request;
    
    $client = new Client();
    
    $requests = function ($total) {
        $uri = 'http://127.0.0.1:8126/guzzle-server/perf';
        for ($i = 0; $i < $total; $i++) {
            yield new Request('GET', $uri);
        }
    };
    
    $pool = new Pool($client, $requests(100), [
        'concurrency' => 5,
        'fulfilled' => function ($response, $index) {
            // this is delivered each successful response
        },
        'rejected' => function ($reason, $index) {
            // this is delivered each failed request
        },
    ]);
    
    // Initiate the transfers and create a promise
    $promise = $pool->promise();
    
    // Force the pool of requests to complete.
    $promise->wait();
    

Hoặc sử dụng một closure mà trả về một promise khi pool gọi closure.
    
    
    $client = new Client();
    
    $requests = function ($total) use ($client) {
        $uri = 'http://127.0.0.1:8126/guzzle-server/perf';
        for ($i = 0; $i < $total; $i++) {
            yield function() use ($client, $uri) {
                return $client->getAsync($uri);
            };
        }
    };
    
    $pool = new Pool($client, $requests(100));
    

## Sử dụng các Responses

In the previous examples, we retrieved a `$response` variable or we were delivered a response from a promise. The response object implements a PSR-7 response, `PsrHttpMessageResponseInterface`, and contains lots of helpful information.

Trong các ví dụ trên, chúng ta đã nhận được biến `$response` hoặc ta nhận được response từ một promise. Đối tượng response sẽ tuân theo một response PSR-7, `PsrHttpMessageResponseInterface`, và chứa rất nhiều thông tin hữu ích.

Chúng ta có thể lấy được code trạng thái và reason phrase của response:
    
    
    $code = $response->getStatusCode(); // 200
    $reason = $response->getReasonPhrase(); // OK
    

Bạn cũng có thể lấy được các header từ response:
    
    
    // Check if a header exists.
    if ($response->hasHeader('Content-Length')) {
        echo "It exists";
    }
    
    // Get a header from the response.
    echo $response->getHeader('Content-Length');
    
    // Get all of the response headers.
    foreach ($response->getHeaders() as $name => $values) {
        echo $name . ': ' . implode(', ', $values) . "rn";
    }
    

Nội dung body của một response có thể được lấy bằng cách sử dụng hàm `getBody`. Body có thể được sử dụng như một string, ép kiểu thành string, hay sử dụng như một luồng giống đối tượng.
    
    
    $body = $response->getBody();
    // Implicitly cast the body to a string and echo it
    echo $body;
    // Explicitly cast the body to a string
    $stringBody = (string) $body;
    // Read 10 bytes from the body
    $tenBytes = $body->read(10);
    // Read the remaining contents of the body as a string
    $remainingBytes = $body->getContents();
    

## Query String Parameters (Tham số trong chuỗi query)

Bạn có thể tạo ra các tham số trong chuỗi query trong một request bằng nhiều cách khác nhau.

Bạn có thể đặt các tham số của chuỗi query trong URI của request:
    
    
    $response = $client->request('GET', 'http://httpbin.org?foo=bar');
    

Bạn có thể chỉ rõ các tham số của chuỗi query bằng cách sử dụng tùy chọn request `query` dưới dạng mảng.
    
    
    $client->request('GET', 'http://httpbin.org', [
        'query' => ['foo' => 'bar']
    ]);
    

Khai báo tùy chọn dưới dạng mảng sẽ sử dụng hàm `http_build_query` của PHP để định dạng chuỗi query. 

Và cuối cùng, bạn cũng có thể tạo được tùy chọn request `query` dưới dạng một chuỗi.
    
    
    $client->request('GET', 'http://httpbin.org', ['query' => 'foo=bar']);
    

## Uploading Data (Đăng tải dữ liệu)

Guzzle cung cấp một số phương thức phục vụ cho việc upload dữ liệu. 

Bạn có thể gửi các request chứa luồng của dữ liệu bằng cách truyền qua chuỗi, resource trả về từ hàm `fopen`, hoặc một thực thể của `PsrHttpMessageStreamInterface` cho tùy chọn request `body`. 
    
    
    // Provide the body as a string.
    $r = $client->request('POST', 'http://httpbin.org/post', [
        'body' => 'raw data'
    ]);
    
    // Provide an fopen resource.
    $body = fopen('/path/to/file', 'r');
    $r = $client->request('POST', 'http://httpbin.org/post', ['body' => $body]);
    
    // Use the stream_for() function to create a PSR-7 stream.
    $body = GuzzleHttpPsr7stream_for('hello!');
    $r = $client->request('POST', 'http://httpbin.org/post', ['body' => $body]);
    

Một cách đơn giản để upload dữ liệu JSON và đặt header phù hợp là sử dụng tùy chọn request `json`:
    
    
    $r = $client->request('PUT', 'http://httpbin.org/put', [
        'json' => ['foo' => 'bar']
    ]);
    

### POST/Form Requests

In addition to specifying the raw data of a request using the `body` request option, Guzzle provides helpful abstractions over sending POST data.

Ngoài việc chỉ rõ dữ liệu thô của một request sử dụng các tùy chọn request `body`, Guzzle cũng cấp các lớp trừu tượng hữu ích trong quá trình gửi dữ liệu POST.

#### Sending form fields (Gửi các trường trong form)

Gửi các request POST `application/x-www-form-urlencoded` yêu cầu chỉ rõ trường POST dưới dạng một mảng trong các tùy chọn request `form_params` 
    
    
    $response = $client->request('POST', 'http://httpbin.org/post', [
        'form_params' => [
            'field_name' => 'abc',
            'other_field' => '123',
            'nested_field' => [
                'nested' => 'hello'
            ]
        ]
    ]);
    

#### Sending form files (Gửi file trong form)

You can send files along with a form (`multipart/form-data` POST requests), using the `multipart` request option. `multipart` accepts an array of associative arrays, where each associative array contains the following keys:

Bạn có thể gửi các file cùng với một form (các `multipart/form-data` POST requests), bằng cách sử dụng tùy chọn request `miultipart`. `multipart` nhận các mảng liên kết, mỗi mảng liên kết sẽ chứa các khóa sau:

* name: (required, string) khóa ánh xạ với trường tên của form
* contents: (required, mixed) Provide a string to send the contents of the file as a string, provide an fopen resource to stream the contents from a PHP stream, or provide a `PsrHttpMessageStreamInterface` to stream the contents from a PSR-7 stream.

Chứa một chuỗi để gửi nội dụng của file dưới dạng một chuỗi, chứa một fopen resource để stream nội dụng của một luồng PHP, hoặc chứa một `PsrHttpMessageStreamInterface` để  stream nội dung từ một luồng PSR-7.
    
    
    $response = $client->request('POST', 'http://httpbin.org/post', [
        'multipart' => [
            [
                'name'     => 'field_name',
                'contents' => 'abc'
            ],
            [
                'name'     => 'file_name',
                'contents' => fopen('/path/to/file', 'r')
            ],
            [
                'name'     => 'other_file',
                'contents' => 'hello',
                'filename' => 'filename.txt',
                'headers'  => [
                    'X-Foo' => 'this is an extra header to include'
                ]
            ]
        ]
    ]);
    

## Cookies

Guzzle có thể duy trì một phiên cookie nếu được yêu cầu sử dụng tùy chọn request `cookies`. Khi gửi một request, tùy chọn `cookies` phải được gắn với một thực thể
 `GuzzleHttpCookieCookieJarInterface`.

    
    // Use a specific cookie jar
    $jar = new GuzzleHttpCookieCookieJar;
    $r = $client->request('GET', 'http://httpbin.org/cookies', [
        'cookies' => $jar
    ]);
    

Bạn có thể gán `cookies` bằng `true` trong client constructor nếu bạn muốn sử dụng shared cookie jar cho tất cả các request.
    
    
    // Use a shared client cookie jar
    $client = new GuzzleHttpClient(['cookies' => true]);
    $r = $client->request('GET', 'http://httpbin.org/cookies');
    

## Redirects (Điều hướng)

Guzzle sẽ tự động điều hướng trừ khi bạn bảo nó không làm vậy. Bạn có thể tùy chỉnh hành đồng điều hướng bằng cách sử dụng tùy chọn request `allow_redirects`.

* Gán thành `true` để bật chế độ điều hướng bình thường với tối đa 5 lần điều hướng. Đây là cài đặt mặc định.
* Gán thành `false` để tắt chức năng điều hướng.
* Pass an associative array containing the 'max' key to specify the maximum number of redirects and optionally provide a 'strict' key value to specify whether or not to use strict RFC compliant redirects (meaning redirect POST requests with POST requests vs. doing what most browsers do which is redirect POST requests with GET requests).

* Truyền một mảng chứa khóa 'max' để chỉ rõ số lần điều hướng tối đa và có thể thêm khóa 'strict' để chỉ rõ rằng liệu điều hướng RFC compliant có được sử dụng hay không (có nghĩa là điều hướng các POST request với các POST so với việc mà hầu hết các trình duyệt sẽ làm đó là điều hướng các POST request với các GET request).
    
    
    $response = $client->request('GET', 'http://github.com');
    echo $response->getStatusCode();
    // 200
    

Ví dụ sau chỉ ra rằng tính năng điều hướng có thể được tắt.
    
    
    $response = $client->request('GET', 'http://github.com', [
        'allow_redirects' => false
    ]);
    echo $response->getStatusCode();
    // 301
    

## Exceptions (Ngoại lệ)

Guzzle sẽ ném các ngoại lệ cho những lỗi xảy ra trong quá trình truyền tải.

* Trong sự kiện lỗi mạng (timeout kết nối, các lỗi DNS, ...), một
 `GuzzleHttpExceptionRequestException` sẽ được ném ra. Ngoại lệ này kế thừa từ  `GuzzleHttpExceptionTransferException`. Việc bắt ngoại lệ này sẽ bắt bất cứ ngoại lệ nào có thể được ném ra trong quá trình truyền tải request.
    
        use GuzzleHttpPsr7;
    use GuzzleHttpExceptionRequestException;
    
    try {
        $client->request('GET', 'https://github.com/_abc_123_404');
    } catch (RequestException $e) {
        echo Psr7str($e->getRequest());
        if ($e->hasResponse()) {
            echo Psr7str($e->getResponse());
        }
    }
    

* Một ngoại lệ `GuzzleHttpExceptionConnectException` được ném ra trong trường hợp lỗi mạng. Ngoại lệ này extends từ  `GuzzleHttpExceptionRequestException`.
* Một `GuzzleHttpExceptionClientException` được ném ra mức lỗi 400 nếu tùy chọn request `http_errors` được gán thành true. Ngoại lệ này kế thừa từ  `GuzzleHttpExceptionBadResponseException` và `GuzzleHttpExceptionBadResponseException` kế thừa từ `GuzzleHttpExceptionRequestException`.
    
        use GuzzleHttpExceptionClientException;
    
    try {
        $client->request('GET', 'https://github.com/_abc_123_404');
    } catch (ClientException $e) {
        echo Psr7str($e->getRequest());
        echo Psr7str($e->getResponse());
    }
    


* Một `GuzzleHttpExceptionServerException` được ném ra cho lỗi mức 500 nếu tùy chọn request `http_errors` được gán thành true. Ngoại lệ này kế thừa từ `GuzzleHttpExceptionBadResponseException`.
* Một `GuzzleHttpExceptionTooManyRedirectsException` sẽ được ném ra khi có quá nhiều lần điều hướng xảy ra. Ngoại lệ này kế thừa từ  `GuzzleHttpExceptionRequestException`.

Tất cả những ngoại lệ trên đều kế thừa từ  `GuzzleHttpExceptionTransferException`.

## Environment Variables (Biến môi trường)

Guzzle cung cấp một vài biến môi trường để có thể sử dụng để tuỳ chỉnh hành động của thư viện.

`GUZZLE_CURL_SELECT_TIMEOUT`
: Controls the duration in seconds that a curl_multi_* handler will use when selecting on curl handles using `curl_multi_select()`. Some systems have issues with PHP's implementation of `curl_multi_select()` where calling this function always results in waiting for the maximum duration of the timeout.

`GUZZLE_CURL_SELECT_TIMEOUT`
: điều chỉnh khoảng thời gian tính bằng giây mà một curl_multi_* handler sử dụng khi chọn curl handles bằng cách sử dụng `curl_multi_select()`. Một vài hệ thống có vấn đề với các triển khai của PHP của `curl_multi_select()` khi mà việc gọi hàm này luôn dẫn tới việc phải chờ trong khoảng thời gian timeout tối đa.

`HTTP_PROXY`
: 

Định nghĩa proxy nào được sử dụng khi gửi các request sử dụng giao thức "http".

Ghi chú: vì biến HTTP_PROXY có thể chứa đầu vào tùy ý từ người dùng nhập vào trên một vài môi trường (CGI), nên nó chỉ được sử dụng trên CLI SAPI. Xem thêm thông tin phía dưới.

`HTTPS_PROXY`
: Định nghĩa proxy nào được sử dụng khi gửi các request mà sử dụng giao thức "https". 

[1]: http://docs.guzzlephp.org/overview.html#installation
[2]: http://tools.ietf.org/html/rfc3986#section-5.2
[3]: http://docs.guzzlephp.org/handlers-and-middleware.html
[4]: https://promisesaplus.com/
[5]: https://github.com/guzzle/promises

  
