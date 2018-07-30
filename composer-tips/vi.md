## 17 lời khuyên để sử dụng Composer hiệu quả

Dù hầu hết các lập trình viên PHP đều biết cách sử dụng Composer, nhưng không phải tất cả đều sử dụng nó một cách hiệu quả hoặc theo một cách tốt nhất có thể.a Vì vậy tôi quyết định sẽ tóm tắt lại những điều quan trọng  mà phục vụ trong công việc hàng ngày của tôi.

Triết lý của hầu hết các lời khuyên là _ "Hãy giữ an toàn" _, có nghĩa là nếu
có nhiều cách khác nhau để xử lý một vấn đề nào đó, tôi sẽ sử dụng cách tiếp cận ít bị lỗi nhất.

## Lời khuyên #1: Hãy đọc documentation (tài liệu)

Tôi thật sự khuyên các bạn nên làm vậy. [Tài liệu chính thức] (https://getcomposer.org/doc/) rất tuyệt và dành ra một vài giờ để đọc nó sẽ giúp bạn tiết kiệm nhiều thời gian sau này. Bạn sẽ ngạc nhiên khi có rất nhiều thứ mà Composer có thể làm.

## Lời khuyên #2: Hãy cảnh giác sự khác biệt giữa một "project" (dự án) và một "library" (thư viện)

Một điều rất quan trọng là bạn phải biết bạn đang tạo một  _"project"_ hay một
_"library"_. Mỗi cái sẽ yêu cầu những công việc khác nhau.

Một library là một package có thể tái sử dụng, ta sẽ thêm chúng như một dependency - ví dụ như `symfony/symfony`, `doctrine/orm` hoặc
[`elasticsearch/elasticsearch`](https://github.com/elastic/elasticsearch-php).

Một project về cơ bản là một ứng dụng, chúng phụ thuộc vào các library. Chúng thường không có tính tái sử dụng (không có project nào sẽ require chúng  như một dependency). Ví tiêu biểu là một website thương mại điện tử, hệ thống hỗ trợ khách hàng,...

Tôi sẽ phân biệt sự khác nhau giữa library và  một project trong các lời khuyên  dưới đây.

## Lời khuyên #3: Sử dụng phiên bản cụ thể của dependency cho ứng dụng

Nếu bạn đang tạo ra một ứng dụng, bạn nên sử dụng phiên bản cụ thể nhất của dependency. Nếu bạn cần parse các file YAML, bạn nên chỉ định
dependency như sau "" symfony / yaml ":" 4.0.2 "`.

Ngay cả những thư viện tuân theo [Quy chuẩn đặt tên phiên bản] (https://semver.org/), những lỗi tương thích với phiên bản cũ vẫn có thể xảy ra trong các phiên bản nhỏ hoặc bản vá . Ví dụ, nếu bạn đang sử dụng `" symfony / symfony ":" ^ 3.1 "`, có thể chính những thứ không được hỗ trợ trong 3.2 sẽ làm hỏng các test ứng dụng của bạn. Hoặc có thể có lỗi đã được sửa trong PHP_CodeSniffer và nó sẽ phát hiện các vấn đề về định dạng mới trong
code, điều này cũng có thể dẫn đến lỗi khi build.

Việc cập nhật các dependency nên được cân nhắc, không được thực hiện một cách vô ý. Một trong những lời khuyên dưới đây sẽ bàn thảo về vấn đề này chi tiết hơn.

Nghe có vẻ quá mức cần thiết, nhưng nó sẽ phòng ngừa việc đồng nghiệp của bạn vô tình cập nhật tất cả các dependency khi thêm một library mới vào project
(điều này có thể xảy ra khi bạn không để ý lúc review lại code).

## Lời khuyên #4: Giới hạn phạm vị phiên bản cho các library dependency

Nếu bạn đang tạo một library, bạn nên xác định giới hạn phiên bản lớn nhất có thể có. Nếu bạn tạo một library có sử dụng library `symfony / yaml` cho YAML
parsing, bạn nên require nó như sau:
    ```
    "symfony/yaml": "^3.0 || ^4.0"
    ```
Điều này có nghĩa rằng thư viện của bạn có thể sử dụng `symfony / yaml` của bất kỳ phiên bản 3 chấm hoặc 4 chấm nào. Điều này quan trọng, bởi vì ràng buộc này được chuyền đến ứng dụng mà sử dụng thư viện của bạn.

Trong trường hợp có hai thư viện có các requirements xung đột nhau, ví dụ: một cái yêu cầu `~ 3.1.0` và cái khác yêu cầu ` ~ 3.2.0`, quá trình cài đặt sẽ thất bại.

## Lời khuyên #5: Nên commit `composer.lock` lên git trong ứng dụng

Nếu bạn đang tạo một _project_, bạn sẽ chắc chắn muốn commit `composer.lock`
lên git. Điều này đảm bảo rằng tất cả mọi người  bao gồm bạn, đồng nghiệp , CI server và production server của bạn sẽ chạy ứng dụng với các phiên bản dependency giống nhau.

Thoạt nhìn, điều này nghe có vẻ không cần thiết - bạn đã sử dụng một
phiên bản cụ thể trong ràng buộc, như đã đề cập trong lời khuyên # 3. Nhưng không, vẫn có các dependency của các dependency của bạn không bị ràng buộc bởi các điều kiện này (ví dụ: `symfony / console` phụ thuộc vào` symfony / polyfill-mbstring`). Vì vậy, nếu không commit `composer.lock`, bạn sẽ có cùng một bộ các dependency giống nhau.

## Lời khuyên #6: Đặt `composer.lock` vào `.gitignore` trong library

Nếu bạn đang tạo một _library_ (hãy tạm gọi là 'acme / my-library`), bạn không nên commit `composer.lock`. Nó [không có bất kỳ ý nghĩa nào] (https://getcomposer.org/doc/02-libraries.md#lock-file) trong các project mà
đang sử dụng thư viện của bạn.

Hãy ví dụ rằng `acme / my-library` sử dụng` monolog / monolog` như một dependency. Nếu bạn đã commit `composer.lock`, mọi người đang phát triển` acme / my-thư viện` sẽ sử dụng phiên bản cũ hơn của Monolog. Nhưng khi library đã hoàn thành và bạn sử dụng nó trong một project thực sự, một phiên bản mới hơn của Monolog có thể được cài đặt và nó có thể không tương thích với library. Do bạn đã không chú ý trước đó, đó là vì `composer.lock`!

Tốt nhất là đặt `composer.lock` vào` .gitignore` của bạn để bạn khỏi vô tình commit nó.

Nếu bạn muốn đảm bảo rằng library tương thích với các các phiên bản khác nhau của dependency của nó, hãy đọc lời khuyên tiếp theo!

## Lời khuyên #7: Chạy Travis CI mà được build với các phiên bản khác nhau của các dependency

> Lời khuyên này chỉ áp dụng đối với các library (bởi bạn sử dụng  các phiên bản cụ thể cho ứng dụng).

Nếu bạn đang xây dựng một thư viện mã nguồn mở, có thể bạn đang sử dụng Travis CI để chạy các bản build của nó.

Mặc định, Composer sẽ cài đặt các phiên bản mới nhất có thể của các dependency mà được khai báo trong `composer.json`. Nó có nghĩa rằng, đối với điều kiện dependency `^ 3.0 || ^ 4.0`, bản build sẽ luôn sử dụng phiên bản mới nhất của bản v4. Do bản 3.0 chưa bao giờ được kiểm thử, thư viện có thể sẽ không tương thích với nó và điều đó sẽ làm cho user của bạn thất vọng.

May mắn thay, Composer cung cấp một tính năng để cài đặt các phiên bản thấp nhất có thể của các dependency `--prefer-low` (nên được sử dụng với` --prefer-stable` để phòng ngừa việc cài đặt các phiên bản không ổn định).

File cấu hình `.travis.yml` được cập nhật có thể sẽ như sau:
    
    language: php
    
    php:
      - 7.1
      - 7.2
    
    env:
      matrix:
        - PREFER_LOWEST="--prefer-lowest --prefer-stable"
        - PREFER_LOWEST=""
    
    before_script:
      - composer update $PREFER_LOWEST
    
    script:
      - composer ci

Hãy xem nó tại [my mhujer/fio-api-php library](https://github.com/mhujer/fio-
api-php / blob / master / .travis.yml) và [the build matrix on Travis
CI](https://travis-ci.org/mhujer/fio-api-php)

Mặc dù giải pháp này sẽ bắt được hầu hết các bản không tương thích, hãy nhớ rằng có nhiều cách kết hợp các dependency giữa phiên bản thấp nhất và mới nhất. Và chúng cũng có thể không tương thích với nhau.


## Lời khuyên #8: Sắp xếp các package trong require và require-dev theo tên

Một thói quen tốt là sắp xếp các package trong `require` và` require-dev` theo tên. Điều này có thể ngăn ngừa những merge conflict không cần thiết khi rebase một nhánh. Bởi nếu bạn đã thêm một package vào cuối danh sách trong hai branch,
sẽ có luôn có xung đột khi merge.

Đây là một công việc nhàm chán khi phải làm thủ công, vì vậy tốt nhất là [cấu hình nó] (https://getcomposer.org/doc/06-config.md#sort-packages) trong
`composer.json`:

    {
    ...
        "config": {
            "sort-packages": true
        },
    ...
    }
    
Lần tới, nếu bạn `require` một package mới, nó sẽ được thêm vào vị trí thích hợp (và không phải ở cuối).

## Lời khuyên #9: Đừng merge `composer.lock` khi đang rebase hoặc merge

Nếu bạn thêm một dependency mới vào `composer.json` (và` composer.lock`) và
trước khi nhánh của bạn được merge, có một dependency khác được thêm vào trong master, bạn cần phải rebase nhánh của bạn. Và bạn sẽ nhận được một merge conflct trong `composer.lock`.

Bạn không bao giờ nên giải quyết xung đột này bằng tay, bởi vì file `composer.lock` chứa một hàm băm của các dependency được định nghĩa trong
`composer.json`. Vì vậy, ngay cả khi bạn giải quyết xung đột, file lock vẫn sẽ không chính xác.

Tốt nhất là tạo `.gitattributes` tại gốc của project bằng dòng sau, nó có nghĩa là git sẽ không merge `composer.lock`:
    
    /composer.lock -merge

Bạn có thể khắc phục vấn đề này bằng cách sử dụng các branch có tính năng ngắn hạn như được đề xuất trong [Trunk Based Development] (https://trunkbaseddevelopment.com/) (thực ra bạn cũng nên thực hành điều này ngay lúc này). Khi bạn có một branch ngắn hạn, được merge ngay lập tức, nguy cơ xảy ra xung đột khi merge trong `composer.lock` được giảm thiểu tối đa. Bạn có thể thậm chí tạo branch chỉ để thêm dependency và merge nó ngay lập tức.

Nhưng phải làm gì nếu bạn gặp phải merge conflict trong file `composer.lock` khi rebase? Hãy giải quyết bằng cách sử dụng phiên bản từ master, nhờ đó những thay đổi sẽ chỉ xuất hiện trong `composer.json` (các package mới được thêm vào). Và sau đó chạy `composer update --lock`, nó sẽ cập nhật file `composer.lock` với các thay đổi từ `composer.json`. Bây giờ bạn có thể stage file đã được `composer.lock` đã được cập nhật và có thể tiếp tục với rebase.

## Lời khuyên #10: Hiểu được sự khác nhau giữa `require` và `require-dev`

Điều quan trọng là ta biết được sự khác biệt giữa `require` và` require-dev`.

Các package cần thiết để chạy ứng dụng hoặc thư viện phải được định nghĩa trong `require` (ví dụ: Symfony, Doctrine, Twig, Guzzle, ...) Nếu bạn đang
tạo một thư viện, hãy cẩn thận về những thứ bạn viêt vào `require`. Bởi vì mỗi
dependency tại phần này cũng là dependency của ứng dụng mà sử dụng thư viện.

Các package cần thiết để phát triển ứng dụng (hoặc thư viện) nên được định nghĩa trong `require-dev` (ví dụ: PHPUnit, PHP_CodeSniffer, PHPStan).

## Lời khuyên #11: Update dependency một cách an toàn

Tôi cho rằng chúng ta có thể nhất trí về việc các dependency nên được update thường xuyên. Những gì tôi muốn thảo luận ở đây là việc update các dependency nên được thực hiện một cách rõ ràng và thận trọng. Không nên thực hiện nó theo kiểu "tiện thể thì làm" kèm với các công việc khác. Nếu bạn sửa đổi một cái gì đó và đồng thời cập nhật một số thư viện, bạn không thể dễ dàng
biết được liệu ứng dụng đã bị hỏng do bạn sửa đổi hay do việc cập nhật đã gây nên.

Bạn có thể sử dụng lệnh `composer outdated` để xem dependency nào có thể
được cập nhật. Bạn cũng có thể thêm tùy chọn `--direct` (hoặc` -D`) để chỉ liệt kê những dependency được khai báo trong `composer.json`. Cũng có một tùy chọn `-m` để liệt kê những phiên bản cập nhật nhỏ.

** Đối với mỗi dependency đã bị lỗi thời, hãy làm theo các bước sau **:

  1. Tạo một branch mới
  2. Cập nhật phiên bản dependency trong `composer.json` thành phiên bản mới nhất
  3. Chạy `composer update phpunit / phpunit --with-dependencies` (thay ` phpunit / phpunit` bằng thư viện bạn đang cập nhật)
  4. Kiểm tra CHANGELOG trong repository thư viện trên Github để xem có bất kỳ thay đổi lớn nào không. Nếu có, hãy cập nhật ứng dụng
  5. Kiểm tra ứng dụng tại local (Nếu bạn đang sử dụng Symfony, bạn có thể thấy các cảnh báo lỗi trong Debug Bar)
  6. Commit các thay đổi (`composer.json`,` composer.lock` và bất cứ điều gì khác cần thiết để phiên bản mới có thể chạy)
  7. Đợi CI build xong
  8. Merge và deploy

Đôi khi, bạn nên cập nhật nhiều dependency cùng một lúc, ví dụ: khi bạn
đang cập nhật Doctrine hoặc Symfony. Trong trường hợp này, bạn có thể liệt kê tất cả trong câu lệnh update sau:
    
    composer update symfony/symfony symfony/monolog-bundle --with-dependencies
    
Hoặc bạn có thể sử dụng ký tự đại diện để cập nhật tất cả các dependency từ một
namespace cụ thể:

    composer update symfony/* --with-dependencies
    
Tôi biết rằng những điều này nghe có vẻ thật tẻ nhạt, nhưng chỉ thỉnh thoảng bạn mới nên cập nhật các dependency, như vậy sẽ an toàn hơn.

Một cách tắt là cập nhật tất cả các `require-dev` dependency cùng một lúc (nếu chúng không yêu cầu thay đổi trong code, nếu không thì tôi đề xuất sử dụng các branch khác nhau để review code dễ dàng hơn).

## Lời khuyên #12: Bạn có thể định nghĩa các kiểu dependency khác trong `composer.json`

Ngoài việc định nghĩa các thư viện như là dependency, bạn cũng có thể định nghĩa những thứ khác ở đó.

Bạn có thể định nghĩa, phiên bản PHP nào mà ứng dụng / thư viện của bạn hỗ trợ:
    
    "require": {
        "php": "7.1.* || 7.2.*",
    },

Bạn cũng có thể xác định extension nào là cần thiết cho ứng dụng / thư viện. Điều này cực kỳ hữu ích khi bạn cố gắng docker hóa ứng dụng của mình hoặc đồng nghiệp mới đang cài đặt ứng dụng lần đầu tiên.

    "require": {
        "ext-mbstring": "*",
        "ext-pdo_mysql": "*",
    },
    
(Bạn nên sử dụng `*` cho các biến thể của phiên bản vì [chúng đôi khi không có tính nhất quán] (https://getcomposer.org/doc/01-basic-usage.md#platform-
packages)).

## Lời khuyên #13: Validate `composer.json` trong quá trình build CI

`composer.json` và` composer.lock` phải luôn được giữ đồng bộ. Vì thế, sẽ thật lý tưởng nếu có thể tự động kiểm tra cho nó. Chỉ cần thêm đoạn này như một phần
của giai đoạn khi bạn build script và nó sẽ đảm bảo rằng `composer.lock` được đồng bộ với `composer.json`:

    composer validate --no-check-all --strict
    
## Lời khuyên #14: Sử dụng một Composer plugin trong PHPStorm

Có một [composer.json plugin cho PHPStorm (https://plugins.jetbrains.com/plugin/7631-php-composer-json-
support). Khi ta sửa `composer.json` bằng tay, autocompletion và một vài validation sẽ được hỗ trợ.

Nếu bạn đang sử dụng IDE khác (hoặc chỉ là một trình soạn thảo code), bạn có thể thiết lập validation đối với [JSON schema của nó] (https://getcomposer.org/schema.json).

## Lời khuyên #15: Chỉ rõ phiên bản PHP production trong `composer.json`

Nếu bạn giống tôi và đôi khi bạn [chạy các phiên bản PHP tiền phát hành ở local] (https://blog.martinhujer.cz/php-7-2-is-due-in-november-whats-new/),
bạn sẽ có nguy cơ cập nhật các dependency tới phiên bản mà sẽ không hoạt động trong production. Ngay lúc này, tôi đang sử dụng PHP 7.2.0, có nghĩa rằng tôi có thể cài đặt thư viện mà không hoạt động trên 7.1. Khi production chạy 7.1,
cài đặt sẽ thất bại.

Nhưng không cần phải lo lắng, có một cách dễ dàng để giải quyết. Chỉ cần chỉ rõ phiên bản PHP ở production trong phần `config` của` composer.json` như sau:

    "config": {
        "platform": {
            "php": "7.1"
        }
    }
    
Đừng nhầm lẫn với phần `require`, chúng hoạt động khác nhau đó. Ứng dụng của bạn có thể chạy trên 7.1 hoặc 7.2 và đồng thời chỉ định 7.1 làm nền tảng (có nghĩa là các dependency sẽ luôn được cập nhật thành phiên bản tương thích với 7.1):

    "require": {
        "php": "7.1.* || 7.2.*"
    },
    "config": {
        "platform": {
            "php": "7.1"
        }
    },
    
## Tip #16: Sử dụng các package private từ  self-hosted Gitlab 

Chúng tôi khuyên bạn nên sử dụng `vcs` làm kiểu của repository và Composer nên xác định cách phù hợp để fetch các package. Ví dụ, nếu bạn thêm một fork từ Github, nó sẽ sử dụng API của nó để tải file .zip thay vì clone toàn bộ repo.

Điều này lại phức tạp hơn đối với một cài đặt cho private Gitlab. Nếu bạn sử dụng `vcs` như một kiểu repo, Composer sẽ phát hiện rằng dó là một cài đặt Gitlab
và sẽ cố tải xuống package bằng API (điều này yêu API key. Do tôi không muốn thiết lập nó, vì vậy tôi đã quyết định thiết lập như sau (sử dụng SSH cho
việc clone):

Đầu tiên chỉ định repo với kiểu `git`:
 
    "repositories": [
        {
            "type": "git",
            "url": "git@gitlab.mycompany.cz:package-namespace/package-name.git"
        }
    ]
    
Sau đó sử dụng package như bình thường:
  
    "require": {
        "package-namespace/package-name": "1.0.0"
    }

## Lời khuyên #17: Làm sao để tạm thời sử dụng một branch với bugfix từ fork

Nếu bạn tìm thấy bug trong một số thư viện public và bạn sửa nó trong fork của bạn trên Github, bạn cần phải cài đặt thư viện từ repo này thay vì từ repo chính gốc (hoặc trừ khi lỗi bug được sửa và merge và phiên bản đã sửa được phát hành).

Điều đó có thể được thực hiện một cách dễ dàng với [inline aliasing (đặt bí danh trên cùng dòng)] (https://getcomposer.org/doc/articles/aliases.md#require-inline-alias):

    {
        "repositories": [
            {
                "type": "vcs",
                "url": "https://github.com/you/monolog"
            }
        ],
        "require": {
            "symfony/monolog-bundle": "2.0",
            "monolog/monolog": "dev-bugfix as 1.0.x-dev"
        }
    }
    
Bạn có thể kiểm tra bản sửa lỗi ở local trước khi push nó bằng cách [sử dụng `path` làm kiểu repo] (https://getcomposer.org/doc/05-repositories.md#path).

## Cập nhật vào ngày 2018-01-08:

Sau khi xuất bản bài viết, tôi đã nhận được đề xuất về một số lời khuyên khác. Sau đây là những lời khuyên đó:

## Lời khuyên #18: Cài đặt prestissimo để tăng tốc độ cài đặt các package

Có một plugin Composer [hirak / prestissimo] (https://github.com/hirak/prestissimo) có thể tăng tốc độ cài đặt dependency bằng cách tải chúng một cách song song.

Và điều tuyệt nhất là gì? Bạn chỉ cần cài đặt nó một cách global một lần, và nó sẽ tự chạy cho tất cả các project khác:

    composer global require hirak/prestissimo

## Lời khuyên #19: Kiểm thử phiên bản nếu bạn chưa chắc chắn

Việc viết các ràng buộc về phiên bản một cách chính xác đôi khi khá khó khăn ngay cả sau khi đọc [tài liệu] (https://getcomposer.org/doc/articles/versions.md#writing-
ràng buộc phiên bản).

May mắn thay, [Packagist Semver Checker] (https://semver.mwl.be/) giúp
bạn có thể kiểm tra phiên bản nào phù hợp với ràng buộc đã chỉ định. Thay vì chỉ
phân tích ràng buộc của phiên bản, nó tải dữ liệu từ Packagist để hiển thị các phiên bản thực tế được phát hành.

Xem [kết quả cho
`symfony / symfony: ^ 3.1`] (https://semver.mwl.be/#?package=symfony%2Fsymfony&version=%5E3.1&minimum-stability=stable).

## Lời khuyên #20: Sử dụng authoritative class map trong production

Bạn nên [tạo authoritative class
map](https://getcomposer.org/doc/articles/autoloader-
optimization.md#optimization-level-2-a-authoritative-class-maps) trong
production. Nó sẽ tăng tốc độ load class bằng cách include mọi thứ trong class-map và bỏ qua các kiểm tra của bất kỳ filesystem nào.

Bạn có thể thực hiện điều này bằng cách chạy lệnh sau như một phần của bản build của production:

    composer dump-autoload --classmap-authoritative
    
## Lời khuyên #21: Cấu hình `autoload-dev` cho các test

Nếu bạn không muốn include các file test trong production class map (bởi lý do về kích thước file và bộ nhớ). Bạn có thể thực hiện điều này bằng cách cấu hình `autoload-dev` (tương tự như `autoload`):
 
    "autoload": {
        "psr-4": {
            "Acme\\": "src/"
        }
    },
    "autoload-dev": {
        "psr-4": {
            "Acme\\": "tests/"
        }
    },
    
## Lời khuyên #22: Hãy thử sử dụng Composer scripts

Composer scripts là một tool nhỏ gon để tạo các build script. Tôi đã viết [một bài riêng về vấn đề này](/have-you-tried-composer-scripts/).
