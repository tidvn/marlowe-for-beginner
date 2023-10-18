Marlowe Step by step

Marlowe có sáu cách xây dựng hợp đồng. Năm trong số này -- `Pay`, `Let`, `If`, `When`và `Assert`-- xây dựng một hợp đồng phức tạp từ các hợp đồng đơn giản hơn và thứ sáu, `Close`, là một hợp đồng đơn giản. Ở mỗi bước thực hiện, cũng như việc trả lại trạng thái mới và hợp đồng tiếp tục, có thể các hiệu ứng -- thanh toán -- và cảnh báo cũng có thể được tạo ra.

## Pay

Hợp đồng thanh toán `Pay a p t v cont`sẽ thực hiện thanh toán giá trị `v` token `t`từ tài khoản `a`cho người nhận thanh toán `p`, người này sẽ là một trong những người tham gia hợp đồng hoặc tài khoản khác trong hợp đồng. Cảnh báo sẽ được tạo nếu giá trị `v`âm hoặc nếu tài khoản không đủ để thanh toán đầy đủ (ngay cả khi có số dư dương của các mã thông báo khác trong tài khoản). Trong trường hợp thứ hai, một khoản thanh toán một phần (tất cả số tiền có sẵn) sẽ được thực hiện. Hợp đồng tiếp tục là hợp đồng được ghi trong hợp đồng: `cont`.

## Close

Một hợp đồng `Close`quy định hợp đồng sẽ được đóng lại (hoặc chấm dứt). Hành động duy nhất mà nó thực hiện là hoàn lại tiền cho chủ sở hữu tài khoản có số dư dương. Việc này được thực hiện một tài khoản mỗi bước, nhưng tất cả các tài khoản sẽ được hoàn trả trong một giao dịch.

## Values, observations and actions

**Các giá trị** bao gồm một số số lượng thay đổi theo thời gian, bao gồm "số vị trí hiện tại", [1](https://docs.marlowe.iohk.io/tutorials/concepts/marlowe-step-by-step#fn-1) "số dư hiện tại của một số mã thông báo trong tài khoản" và bất kỳ lựa chọn nào đã được thực hiện; chúng tôi gọi những giá trị *dễ thay đổi* này . Các giá trị cũng có thể được kết hợp bằng cách sử dụng phép cộng, phép trừ, phủ định, nhân và chia và có thể có điều kiện đối với một quan sát. Mặc dù chúng được Marlowe hỗ trợ, việc sử dụng phép nhân và chia có thể khiến quá trình phân tích tĩnh trở nên khó khăn.

**Các quan sát** là các giá trị Boolean thu được bằng cách so sánh các giá trị và có thể được kết hợp bằng cách sử dụng các toán tử Boolean tiêu chuẩn. Cũng có thể quan sát xem liệu có bất kỳ lựa chọn nào đã được thực hiện hay không (đối với một lựa chọn cụ thể đã được xác định).

Các quan sát sẽ có giá trị ở mỗi bước thực hiện. Mặt khác, **các hành động** xảy ra tại các điểm cụ thể trong quá trình thực hiện. Như đã lưu ý trước đó, các hành động có thể

- gửi tiền,
- đưa ra lựa chọn giữa các lựa chọn thay thế khác nhau, bao gồm giá trị *oracle* (xem phần tiếp theo) 
- thông báo một giá trị bên ngoài của một số loại.

## Oracles

Oracle dành cho blockchain **là các dịch vụ có chức năng như cầu nối giữa các blockchain và thế giới thực, cho phép các hợp đồng thông minh truy cập các nguồn dữ liệu bên ngoài mạng gốc của chúng**. Nói cách khác, chúng hoạt động như một liên kết giữa các ứng dụng trên chuỗi và dữ liệu ngoài chuỗi.

![Tìm hiểu Oracle trong Blockchain - Kiến thức đầu tư tiền điện tử](https://haimanh.vn/Uploaded_products/img_any/ung-dung-cua-oracle-trong-crypto.jpg)

## If

Điều kiện `If obs cont1 cont2`sẽ tiếp tục dưới dạng `cont1`hoặc `cont2`, tùy thuộc vào giá trị Boolean của quan sát `obs`khi cấu trúc này được thực thi.

## When

Đây là hàm tạo phức tạp nhất cho các hợp đồng, có dạng `When cases timeout cont`. Đó là một hợp đồng được *kích hoạt dựa trên các hành động* , có thể xảy ra hoặc không xảy ra tại bất kỳ vị trí cụ thể nào: điều gì xảy ra khi các hành động khác nhau xảy ra được mô tả theo các *trường hợp* trong hợp đồng.

Để đảm bảo rằng hợp đồng cuối cùng sẽ đạt được tiến triển, hợp đồng `When cases timeout cont`sẽ tiếp tục như `cont`sau khi `timeout`đạt được số vị trí.

## Let

Hợp đồng let `Let id val cont`cho phép hợp đồng đặt *tên cho một giá trị* bằng cách sử dụng mã định danh. Trong trường hợp này, biểu thức `val`được đánh giá và lưu trữ với tên `id`. Hợp đồng sau đó tiếp tục như `cont`.

## Assert

Tài sản ảo (Crypto assets/digital assets): Đây **là khái niệm nói chung nhằm chỉ tất cả các loại tài sản được sinh ra trong cuộc cách mạng blockchain và sử dụng công nghệ mật mã hóa**. Cả tiền ảo lẫn tokens ảo đều nằm trong danh mục này.

Hợp đồng khẳng định `Assert obs cont`không có bất kỳ ảnh hưởng nào đến trạng thái của hợp đồng, nó ngay lập tức tiếp tục như `cont`, nhưng nó đưa ra cảnh báo khi quan sát `obs`là sai. Nó có thể được sử dụng để đảm bảo rằng một thuộc tính được giữ ở bất kỳ điểm nhất định nào của hợp đồng, vì phân tích tĩnh sẽ thất bại nếu bất kỳ việc thực thi nào gây ra cảnh báo.
