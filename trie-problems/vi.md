
[Source](https://threads-iiith.quora.com/Tutorial-on-Trie-and-example-problems "Permalink to Tutorial on Trie and example problems - Threads @ IIIT Hyderabad")

# Hướng dẫn về Trie và ví dụ về các vấn đề liên quan

Trong bài viết lần này tôi sẽ nói về Tries và khái niệm được được sử trong các vấn đề có tính biến hóa, linh động. Chúng ta sẽ xem xét 2-3 vấn đề mà tại đó trie có ích.

First we see what a trie is. Trie can store information about keys/numbers/strings compactly in a tree.
Trước tiên chúng ta sẽ xem trie là gì. Trie có thể lưu trữ thông tin về các khóa/số/chuỗi một cách gọn nhẹ trong một cây. 
Trie bao gồm các node, mỗi node lưu trữ một kí tự/bit. Chúng ta có thể thêm các chuỗi/số mới vào một cách phù hợp.

Dưới đây là một ví dụ về trie chứa các chuỗi:

![][1]

  
Nguồn: Wikipedia.

Nhưng ở đây chúng ta sẽ làm việc với các con số, cụ thể là các bit nhị phân. Chúng ta sẽ rõ khi giải quyết các vấn đề này.

**Vấn đề 1**: Cho một mảng các số nguyên, hãy tìm hai phần tử mà khi thực hiện phép XOR lên chúng cho ra giá trị lớn nhất. 
**Giải pháp:**  
Giả sử chúng ta có cấu trúc dữ liệu thỏa mãn hai kiểu truy vấn sau:
1\. Chèn thêm số X
2\. Cho Y, tìm max của phép tính XOR của Y với tất cả các số mà đã được thêm tới giờ.

Nếu chúng ta có cấu trúc dữ liệu kiểu như vậy, ta sẽ lần lượt chèn các số nguyên, và với truy vấn của kiểu thứ hai ta sẽ tìm được max của phép tính XOR.
Trie là cấu trúc dữ liệu mà ta sẽ sử dụng. Trước tiên, hãy xem cách mà ta chèn các phần tử vào trie.

![][2]


Như vậy, ta sẽ lần theo đường đi của các số mà ta cần chèn, chúng tôi sẽ không vẽ lại đường đi nữa. 

Phép chèn khóa có độ dài N tốn O(N) tương đương log2(MAX), trong đó MAX là giá trị lớn nhất được chèn vào trie, bởi có nhiều nhất log2(MAX) bit nhị phân trong một số.
Với cách này, chúng ta lưu trữ được tất cả dữ liệu về tất cả các số mà đã được chèn vào trong trie cho tới thời điểm hiện tại.
Bây giờ, với truy vấn kiểu thứ hai:
Giả sử số Y là b1, b2, ... bn, với b1, b2, ... là các bit nhị phân. Ta sẽ bắt đầu từ b1. Giờ để phép XOR có giá trị lớn nhất, ta sẽ cố gắng đạt được nhiều bit 1 nhất sau khi thực hiện phép XOR. Do vậy, nếu b1 bằng 0, ta sẽ cần một bit 1 và ngược lại. Trong trie, ta sẽ đi theo hướng của bit được yêu cầu tiếp theo. Thực hiện cách này với i từ 1 tới n, ta sẽ có giá trị lớn nhất của phép XOR.


![][3]

Truy vấn hai mất log2(MAX).

**Vấn đề 2**: Cho một mảng các số nguyên, tìm mảng con có giá trị XOR lớn nhất. 
**Giải pháp:**  
Giả sử F(L,R) là XOR của mảng con từ L tới R.
Ở đây ta sử dụng tính chất F(L,R) = F(1,R) XOR F(1,L-1). Tại sao lại như vậy?
Giả sử mảng con với XOR lớn nhất kết thúc tại vị trí thứ i. Giờ ta cần tối đa giá trị F(L, i). F(L, i) XOR F(1,L-1) với L<=i. Giả sử ta đã chèn F(1,L-1) vào trie với mỗi L<=i, lúc đó bài toán sẽ quay về vấn đề 1.

    
    
    ans=0
    pre=0
    Trie.insert(0)
    for i=1 to N:
        pre = pre XOR a[i]
        Trie.insert(pre)
        ans=max(ans, Trie.query(pre))
    print ans
    

Bạn có thử vấn đề này tại: [ACM-ICPC Live Archive][4]

**Vấn đề 3**: Cho một mảng các số nguyên dương, hãy in số lượng các mảng con mà XOR của chúng nhỏ hơn K.
**Giải pháp:**  
Giải pháp lần này một lần nữa sử dụng các khái niệm mà ta đã thấy cho tới giờ. Ta sẽ làm như câu hỏi trước. Với mỗi index i=1 tới N, ta có thể đếm số các mảng con kết thúc tại vị trí thứ i mà thỏa mã điều kiện.

    
    
    ans=0
    p=0
    for i=1 to N:
        q=p XOR A[i]
        ans += Trie.query(q,k)
        Trie.insert(q)
        p=q
    

query(q,k) trả về số lượng các số nguyên đã tồn tại trong cấu trúc mà khi ta thực hiện phép XOR chúng với q sẽ trả về số nguyên nhỏ hơn k.
Ta so sánh bit tương ứng của q và k, bắt đầu từ bit lớn nhất. Giả sử p và q là hai bit mà ta đang xem xét.

Nếu q bằng 1, p bằng 0, ta sẽ thực hiện như sau:

![][5]

Tương tự ta có thể rất đơn giản giải 3 trường hợp khác. (q=1,p=1), (q=0,p=1) and (q=0,p=1).

Do vậy, ta cần thay đổi cấu trúc hiện tại, ta cũng sẽ giữ số lượng các node lá mà có thể đi tới từ node hiện tại nếu ta đi từ phía bên trái và tương tự đối với phía bên phải. Bởi nếu không độ phức tạp sẽ tăng nếu ta lặp đi lặp lại việc duyệt cây. Ta có thể thực hiện điều này trong khi chèn các số vào cây một cách rất dễ dàng.

Vấn đề này được nêu ra tại CodeCraft'14. Bạn có thể thực hành tại: [SPOJ.com - Problem SUBXOR][6]

Giờ, hãy xem xét về cách triển khia code.
Để triển khai code cho trie trong C/CPP ta có thể giữ các node và các con trỏ trái và phải. Ta có thể viết hàm đệ quy.   

    
    
    insert(root, num, level):
        if level==-1: return root
        x=level'th bit of num
        if x==1:
            if root->right is NULL: create root->right
            else: insert(root->right,num,level-1)
        else:
            if root->left is NULL: create root->left
            else: insert(root->left,num,level-1)
    

  
Đối với các truy vấn, ta duyệt một cách đệ quy trên cây.

Cập nhật:
Một vấn đề khác cũng sử dụng Trie(yay! :P).  
[Problem - B - Codeforces][7]

**Vấn đề con**: Cho một nhóm gồm _n_ chuỗi không rỗng. Trong trò chơi ta đang xét, hai người chơi cùng xây dựng từ cùng nhau, khởi đầu thì từ rỗng. Người chơi lần lượt thay nhau chơi. Với mỗi lượt, người chơi phải thêm một chữ cái vào cuối của từ, kết quả của từ phải là tiền tố của ít nhất một chuỗi từ nhóm. Một người chơi sẽ thua nếu anh ta không thể thực hiện bước đi tiếp theo của mình.

Ta cần tìm người chơi nào (1 hoặc 2) mà có khả năng chiến thắng.

Vậy, ý tưởng ở đây lần nữa là xây dựng một trie chứa các chuỗi. Tại sao? Bởi một trie luôn lưu trữ thông tin về tất cả các tiền tố.
Bây giờ, ta sẽ thử đánh giá xem với mỗi node liệu người chơi đầu tiên có khả năng giành thắng lợi hay không. Ta có thể thực hiện điều này một cách đệ quy. Với node v, mỗi node u mà u là con trực tiếp của v, nếu người chơi đầu tiên có khả năng thua vì u, thì với node v người chơi đầu sẽ có khả năng giành chiến thắng.
Ví dụ, giả sử ta có "abc", "abd", "acd".
Trie của chúng ta sẽ nhìn như sau:

![][8]

  
Tất cả các node lá đều có khả năng chiến thắng.

[1]: https://qph.fs.quoracdn.net/main-qimg-aea28d9cd34aaf2d5783f4cd04e5abbd
[2]: https://qph.fs.quoracdn.net/main-qimg-388217a1992f1b2aac51e9917aa76d9c
[3]: https://qph.fs.quoracdn.net/main-qimg-e5d624e2cd693d713840a30ca9aaa461
[4]: https://icpcarchive.ecs.baylor.edu/index.php?Itemid=8&category=345&option=com_onlinejudge&page=show_problem&problem=2683
[5]: https://qph.fs.quoracdn.net/main-qimg-f24ea5ecf11805e7bcd82a48bb9cad25
[6]: http://www.spoj.com/problems/SUBXOR
[7]: http://codeforces.com/contest/455/problem/B
[8]: https://qph.fs.quoracdn.net/main-qimg-f81def67dffcc9e95306d65b27daa2f7-c

  
