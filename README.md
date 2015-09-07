# sed-command
Ngày hôm nay tìm hiểu về sed, bỏ quả một mớ trình bày loăng ngoằng nhưng vẫn phải đọc cho biết.

Theo tôi thì sed đúng như tên gọi của nó : stream editor 

Ban đầu tôi khi được giao về tìm hiểu tôi nghĩ nó là một tiện ích tìm kiếm giống như grep, nhưng trong quá trình tìm hiểu thì thấy nó dùng để sửa văn bản nhiều hơn, nhưng rôi tôi lại nghĩ thế này thì làm quái vì phải dùng sed làm gì cho mệt và mất thời gian vì ta có thể dừng vi hay vim hay một tiện ích chỉnh sửa nào đó cho tiện có phải hơn không.

Nhưng rôi tôi nhận ra là nó có thế mạnh của riêng nó nhất là khi ta chỉ sử dụng giao diện dòng lệnh và một vài tính năng mà các trình chỉnh sửa văn bản không có.  Thôi không lan man nữa, cùng đi vào tìm hiểu các thê mạnh đó nào:

1) s (substitution)

lệnh substitute: s. Lệnh substitute có tác dụng thay đổi toàn bộ nội dung của một biểu thức thành một biểu thức mới

cú pháp: $ sed  's/pattern/replace_string/' file   hoặc $ cat file | sed  's/pattern/replace_string/'

trong đó là pattern là  biểu thức mẫu để tìm kiếm trong input nếu có sẽ được thay thế bởi biểu thức mới. Trong 1 văn bản, chuỗi cần thay thế (pattern) có thể xuất hiện từ 0 đến nhiều lần, mỗi lần như vậy được gọi là 1 xuất hiện của chuỗi cần thay thế.

2) Ký tự slash (/) tương đương như một dấu phân cách (delimiter)

Ký tự (/) sau lệnh s có vai trò như một ký tự phân cách. Bạn không nhất thiết phải sử dụng slash(/) mà thay vào đó có thể sử dụng bất cứ ký tự đặc biệt nào mà bạn muốn. Khi ta muốn thay đổi 1 đoạn nào đó mà lại có slash(/) trong đó,ví dụ: /usr/bin thành /usr/local chẳng hạn --  bạn có thể sử dụng baskslash (\) để chỉ rõ ra ký tự slash (/) trong trường hợp này không có vai trò là ký tự phân cách của câu lệnh nào cả. Nhưng tôi khuyên bạn không nên dùng kiểu này vì nó khá là đau mắt, tôi đề nghị 1 vài kiểu dễ nhìn hơn -- ví dụ:

sed 's_/usr/bin_/usr/local_'

hoặc

sed 's:/usr/bin:/usr/local:'

3) Lưu các thay đổi vào tập tin

Để lưu các thay đổi trực tiếp vào 1 file ta sử dụng tùy chọn -i :

`$ sed -i 's/text/replace/' file`

4) Thay thế tất cả xuất hiện của mẫu

Nếu chúng ta sử dụng các cú pháp đã đề cập ở trên, sed sẽ thay thế sự xuất hiện đầu tiên của mẫu (pattern) trong mỗi dòng, ví dụ:

Giả sử bạn có input file:
```sh
one two three, one two three
four three two one
one hundred
```
bạn sử dụng lệnh

` sed 's/one/ONE/' <file `

Bạn được output
```sh
ONE two three, one two three
four three two ONE
ONE hundred
```
Nếu chúng ta muốn thay thế tất cả xuất hiện của mẫu trong văn bản, chúng ta cần thêm tham số g vào cuối như sau:

`$  sed 's/one/ONE/g' <file ``

tham số g sẽ thay thế tất cả các dòng có one thành ONE .

Tuy nhiên, đôi khi chúng ta không muốn thay thế tất cả xuất hiện của mẫu, mà chỉ muốn thay thế từ xuất hiện thứ N của mẫu cho đến cuối văn bản. Để làm việc này, chúng ta có thể sử dụng dạng /Ng như sau:

`$ echo 1111 | sed 's/1/2/2g' `
--> 122
` $ echo 1111 | sed 's/1/2/3g' `
---> 1122

Nếu chúng ta chỉ muốn thay thế xuất hiện thứ N của mẫu trong văn bản, sử dụng dạng /N như sau:

`$ echo 1111 | sed 's/1/2/2' `
--> 1211

`$ echo 1111 | sed 's/1/2/3' `
--> 1131


5) Xóa các dòng trống

Xóa các dòng trống là 1 kỹ thuật đơn giản với việc sử dụng sed. Các khoảng trống có thể được đối chiếu với biểu thức chính quy ^$:

` $ sed '/^$/d' file `






