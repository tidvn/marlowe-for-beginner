# TheMarlowePlayground

Hướng dẫn này cung cấp cái nhìn tổng quan về [Marlowe Playground](https://play.marlowe.iohk.io/#/) , một công cụ trực tuyến cho phép người dùng tạo, phân tích, tương tác và mô phỏng hoạt động của các hợp đồng Marlowe được nhúng.

## Introducing the Marlowe Playground

Để Marlowe có thể sử dụng được trong thực tế, người dùng cần có khả năng hiểu các hợp đồng sẽ hoạt động như thế nào sau khi được triển khai trên blockchain nhưng không thực hiện việc triển khai. Chúng tôi có thể làm điều đó bằng cách mô phỏng hành vi của họ ngoài chuỗi, tương tác từng bước thông qua việc đánh giá hợp đồng trong công cụ dựa trên trình duyệt, Marlowe Playground, một công cụ web hỗ trợ xây dựng, sửa đổi và mô phỏng tương tác các hợp đồng thông minh được viết bằng Marlowe.

Hợp đồng có thể được soạn thảo theo bốn cách khác nhau trong Sân chơi. Chúng ta có thể viết trực tiếp bằng Marlowe hoặc sử dụng cách biểu diễn Blockly của Marlowe. Marlowe cũng được nhúng trong Haskell và JavaScript và chúng tôi có thể soạn thảo hợp đồng bằng các ngôn ngữ này rồi chuyển đổi ("biên dịch") chúng thành Marlowe trong Playground. Khi hợp đồng đã được viết bằng Blockly, Haskell hoặc JavaScript, chúng ta có thể chuyển sang Trình mô phỏng để phân tích và mô phỏng hợp đồng.

## Getting started

Trang đích của Sân chơi Marlowe trông như thế này:

![Trang đích của Sân chơi](https://docs.marlowe.iohk.io/assets/images/landing-page-0309a3014ca31778db10223778f10e49.png)

Thanh tiêu đề có liên kết [Marlowe Playground](https://play.marlowe.iohk.io/#/) đến hướng dẫn này ở phía bên phải và chân trang có một số liên kết chung.

Trang này cung cấp ba tùy chọn:

- **Mở dự án hiện có Thao tác** này sẽ mở một dự án đã được lưu trước đó. Xem phần [Lưu và mở dự án](https://docs.marlowe.iohk.io/tutorials/concepts/playground-overview#saving-and-opening-projects) bên dưới để biết thêm chi tiết về cách thiết lập dự án này.
- **Mở một ví dụ Thao tác** này sẽ tải một ví dụ vào dự án hiện có, trong môi trường do người dùng chọn.

![Mở một ví dụ](https://docs.marlowe.iohk.io/assets/images/open-example-3831546c7b90121879cb647223370511.png)

- **Bắt đầu điều gì đó mới** Tại đây, bạn được lựa **chọn** bắt đầu bằng Javascript, Haskell, Marlowe hoặc Blockly.
- Mỗi lựa chọn này đều được đề cập ngay bây giờ.
- Dù bạn bắt đầu ở đâu, bạn sẽ có cơ hội **mô phỏng** các hợp đồng mà bạn phát triển.

Trình chỉnh sửa chương trình được sử dụng trong Playground là trình chỉnh sửa Monaco -- [Monaco Editor](https://microsoft.github.io/monaco-editor/) -- và nhiều tính năng của nó có sẵn, bao gồm cả menu có sẵn khi nhấp chuột phải.

## The JavaScript editor: developing embedded contracts

Chúng ta có thể sử dụng JavaScript để làm cho các định nghĩa hợp đồng dễ đọc hơn bằng cách sử dụng các định nghĩa JS cho các thành phần phụ, chữ viết tắt và các hàm mẫu đơn giản. Trình soạn thảo JS được hiển thị ở đây.

![Trình soạn thảo JavaScript](https://docs.marlowe.iohk.io/assets/images/js-editor-80135712707680ee63ba54b501bec4d5.png)

Trình soạn thảo JS được mở ở đây trên *Escrow với* ví dụ tài sản thế chấp có trong các ví dụ. Để mô tả hợp đồng Marlowe trong trình soạn thảo, giá trị của loại `Contract`phải được trả về do hàm được cung cấp bằng cách sử dụng lệnh `return`.

![Giá trị được trả về bởi ``return`` xác định hợp đồng.](https://docs.marlowe.iohk.io/assets/images/js-editor-cont-082793588f3220b18dc6acc55f9d6dd6.png)

Trình chỉnh sửa hỗ trợ tự động hoàn tất, kiểm tra lỗi trong quá trình chỉnh sửa và thông tin về các ràng buộc khi di chuột qua. Cụ thể, việc sử dụng chuột trên bất kỳ liên kết đã nhập nào sẽ hiển thị loại của nó (trong TypeScript).

Các nút trong thanh tiêu đề cung cấp chức năng tiêu chuẩn:

- Tạo một **dự án mới**
- **Mở** một dự án hiện có
- **Mở** một trong **những ví dụ được tích hợp sẵn**
- **Đổi tên** dự án hiện có
- **Lưu** dự án hiện tại dưới tên hiện tại của nó (nếu có)
- **Lưu** dự án hiện tại **dưới dạng** dự án mới được đặt tên

Khi bạn nhấp vào nút **Biên dịch** (ở góc trên cùng bên phải), mã trong trình chỉnh sửa sẽ được thực thi và đối tượng JSON được hàm trả về từ quá trình thực thi sẽ được phân tích cú pháp thành hợp đồng Marlowe thực tế; sau đó bạn có thể nhấn **Gửi tới trình mô phỏng** để bắt đầu mô phỏng hợp đồng.

Nếu biên dịch thành công, mã đã biên dịch sẽ hiển thị bằng cách chọn **Mã đã tạo** ở chân trang; điều này sau đó cũng có thể được giảm thiểu.

![Mã JS được biên dịch sang Marlowe](https://docs.marlowe.iohk.io/assets/images/js-compiled-4fcb291dfc536fd025f0e11dd7a4cf62.png)

Nếu hợp đồng không thể được chuyển đổi thành công sang Marlowe thì lỗi cũng được hiển thị trong lớp phủ có thể truy cập được dưới dạng **Lỗi** ở chân trang.

![Lỗi khi biên dịch mã JS sang Marlowe](https://docs.marlowe.iohk.io/assets/images/js-error-d1e62aa2935dc5a1e84030b361d804cd.png)

Nhìn lại phần chân trang, bạn có thể thấy rằng bạn có thể truy cập các kết quả **Phân tích tĩnh** , như được mô tả bên dưới, cũng như kiểm tra và chỉnh sửa **Siêu dữ liệu** hợp đồng .

![Siêu dữ liệu cho hợp đồng JavaScript Marlowe](https://docs.marlowe.iohk.io/assets/images/js-metadata-a3760d47657b7ef1270e15768c2e8f96.png)

Trình chỉnh sửa siêu dữ liệu chứa các trường mô tả chung cho mọi hợp đồng nhưng cũng hỗ trợ nhập thông tin mô tả những điều này:

- vai trò
- thông số
- sự lựa chọn

Khi hợp đồng được biên soạn thành công, trình chỉnh sửa siêu dữ liệu sẽ nhắc bạn thêm siêu dữ liệu cho các trường tương ứng với các thành phần thích hợp của hợp đồng và xóa các trường không tương ứng với bất kỳ nội dung nào trong hợp đồng.



## The Haskell editor: developing embedded contracts

Trình chỉnh sửa hỗ trợ phát triển các hợp đồng Marlowe được mô tả trong Haskell. Chúng ta có thể sử dụng Haskell để làm cho các định nghĩa hợp đồng dễ đọc hơn bằng cách sử dụng các định nghĩa Haskell cho các thành phần phụ, chữ viết tắt và các hàm mẫu đơn giản. Trình chỉnh sửa Haskell được hiển thị trong hình ảnh sau đây.

![Trình soạn thảo Haskell](https://docs.marlowe.iohk.io/assets/images/haskell-editor-776e56c16df1ecf311ac3a35bb76396e.png)

Trình chỉnh sửa hỗ trợ tự động hoàn tất, kiểm tra lỗi trong quá trình chỉnh sửa và thông tin về các ràng buộc khi di chuột qua. Các nút trong thanh tiêu đề cung cấp chức năng tiêu chuẩn:

- Tạo một **dự án mới**
- **Mở** một dự án hiện có
- **Mở** một trong **những ví dụ được tích hợp sẵn**
- **Đổi tên** dự án hiện có
- **Lưu** dự án hiện tại dưới tên hiện tại của nó (nếu có)
- **Lưu** dự án hiện tại **dưới dạng** dự án mới được đặt tên

Trình soạn thảo Haskell được mở ở đây trên ví dụ Escrow có trong các ví dụ. Để mô tả hợp đồng Marlowe trong trình chỉnh sửa, chúng ta phải xác định giá trị cấp cao nhất `contract`của loại `Contract`; chính giá trị này được chuyển đổi thành Marlowe thuần túy bằng nút **Biên dịch** (ở góc trên cùng bên phải). Nếu biên dịch thành công, mã đã biên dịch sẽ hiển thị bằng cách chọn **Mã đã tạo** ở chân trang:

![Mã Haskell được biên dịch sang Marlowe](https://docs.marlowe.iohk.io/assets/images/haskell-compiled-7b96951f69b0cfe2d67c08098285b6e2.png)

Khi biên dịch thành công, kết quả có thể được gửi đến trình mô phỏng hoặc tới Blockly: các tùy chọn này được cung cấp bởi các nút **Gửi đến Trình mô phỏng** và **Gửi tới Blockly** ở góc trên cùng bên phải của trang.

Nếu hợp đồng không thể được chuyển đổi thành công sang Marlowe, các lỗi cũng được hiển thị bằng cách chọn **Lỗi** ở chân trang:

![Lỗi khi biên dịch mã Haskell sang Marlowe](https://docs.marlowe.iohk.io/assets/images/haskell-errors-f8c2b8b658941e87a7d43e98357519c6.png)

Nhìn lại phần chân trang, bạn có thể thấy rằng bạn có thể truy cập các kết quả **Phân tích tĩnh** , như được mô tả bên dưới, cũng như kiểm tra và chỉnh sửa **Siêu dữ liệu** hợp đồng .

![Siêu dữ liệu cho hợp đồng Haskell Marlowe](https://docs.marlowe.iohk.io/assets/images/haskell-metadata-86cc91b86d691af8674666d2a9eba9ba.png)

Trình chỉnh sửa siêu dữ liệu chứa các trường mô tả chung cho mọi hợp đồng nhưng cũng hỗ trợ nhập thông tin mô tả những điều này:

- vai trò
- thông số
- sự lựa chọn

Khi hợp đồng được biên soạn thành công, trình chỉnh sửa siêu dữ liệu sẽ nhắc bạn thêm siêu dữ liệu cho các trường tương ứng với các thành phần thích hợp của hợp đồng và xóa các trường không tương ứng với bất kỳ nội dung nào trong hợp đồng.

Siêu dữ liệu hợp đồng không chỉ cung cấp tài liệu cho các hợp đồng Marlowe mà còn được sử dụng trong ứng dụng web giao diện người dùng, ứng dụng khách người dùng cuối sẽ được sử dụng để chạy các liên hệ của Marlowe trên chuỗi khối Cardano kết hợp với Marlowe Runtime.



## The Blockly editor

Trình chỉnh sửa Blockly cung cấp cơ chế tạo và xem hợp đồng ở dạng trực quan thay vì ở dạng văn bản.

Lưu ý rằng trình chỉnh sửa Blockly cũng cung cấp quyền truy cập vào trình chỉnh sửa siêu dữ liệu và phân tích tĩnh.



## Developing contracts in Marlowe

Cũng có thể tạo hợp đồng ở Marlowe "thô". Marlowe được chỉnh sửa trong trình chỉnh sửa Marlowe và điều này cung cấp định dạng tự động (khi nhấp chuột phải) và cũng hỗ trợ **các lỗ hổng** .

![Chỉnh sửa Marlowe: sử dụng lỗ](https://docs.marlowe.iohk.io/assets/images/marlowe-editor-269262b31168ddcb3e7d2f8516a0b93b.png)

Các lỗ cho phép chương trình được xây dựng từ trên xuống. Nhấp vào bóng đèn bên cạnh một lỗ sẽ hiển thị menu hoàn thành, trong mỗi trường hợp, thay thế từng thành phần phụ bằng một lỗ mới. Ví dụ: chọn `Pay`lấp đầy lỗ cấp cao nhất sẽ dẫn đến điều này (sau khi định dạng bằng cách nhấp chuột phải):

![Chỉnh sửa Marlowe: lấp đầy một lỗ hổng](https://docs.marlowe.iohk.io/assets/images/marlowe-hole-fill-3fbea3bdf76b5875f9a6823517cc2616.png)

Các lỗ có thể được kết hợp với chỉnh sửa văn bản thông thường để bạn có thể sử dụng kết hợp các cấu trúc từ dưới lên và từ trên xuống trong việc xây dựng hợp đồng Marlowe. Hơn nữa, các hợp đồng có lỗ có thể được chuyển đến và đi từ Blockly: các lỗ ở Marlowe trở thành các lỗ theo đúng nghĩa đen trong Blockly. Để chuyển sang Blockly, hãy sử dụng **Xem dưới dạng khối** ở góc trên bên phải màn hình và *ngược lại* .



## Simulating Marlowe contracts and templates

Tuy nhiên, hợp đồng được viết ra, khi nó được gửi tới mô phỏng thì đây là chế độ xem được nhìn thấy đầu tiên. Ở đây chúng ta đang xem ví dụ về *trái phiếu phiếu giảm giá Zero* .

![Khung mô phỏng](https://docs.marlowe.iohk.io/assets/images/simulation-tab-5f978d1eeaf28e731a236592af6e3101.png)

Trước khi bắt đầu mô phỏng, bạn cần cung cấp một số thông tin.

- *Thời gian ban đầu* được mô phỏng để bắt đầu mô phỏng.
- Bất kỳ *tham số thời gian chờ nào* : ở đây chúng tôi đưa ra thời gian mà người cho vay phải gửi số tiền đó và thời gian mà người đi vay cần hoàn trả số tiền đó cùng với lãi suất.
- Bất kỳ *tham số giá trị* nào : trong trường hợp này là số tiền đã vay và số tiền lãi (thêm) phải trả.

Mã hiển thị ở đây trình bày hợp đồng hoàn chỉnh đang được mô phỏng. Khi quá trình mô phỏng đã bắt đầu, bất kỳ nội dung nào của hợp đồng còn được mô phỏng sẽ được đánh dấu. Phần chân trang cung cấp dữ liệu về mô phỏng.

Ví dụ của chúng tôi, hãy điền các tham số như thế này.

![Đã thêm thông số.](https://docs.marlowe.iohk.io/assets/images/completed-params-237eb9b4aeb5ce85e22d2721270dfd09.png)

Quá trình mô phỏng được bắt đầu bằng cách nhấp vào nút **Bắt đầu mô phỏng** và khi việc này hoàn tất, các hành động sẵn có sẽ thúc đẩy hợp đồng sẽ được trình bày. Cũng lưu ý rằng toàn bộ hợp đồng được đánh dấu, cho thấy rằng chưa có hợp đồng nào 

được thực thi.

![Các hành động có sẵn](https://docs.marlowe.iohk.io/assets/images/available-actions-b87e90cad9cdd11280b8334ea742c9de.png)

Trong trường hợp này, có 4 hành động tiềm năng: *Người cho vay* có thể đặt cọc 10.000 Ada hoặc thời gian có thể chuyển sang phút tiếp theo, thời gian chờ tiếp theo (trong trường hợp này là thời gian chờ của Khoản *vay* mà chúng tôi vừa đặt, lúc đó thời gian chờ đối với khoản tiền gửi hết thời gian) hoặc trực tiếp đến thời điểm hết hạn của hợp đồng. Hai hành động chung khác cũng có thể được thực hiện:

- **Hoàn tác** sẽ hoàn tác hành động cuối cùng được thực hiện trong trình mô phỏng. Điều này có nghĩa là chúng ta có thể khám phá hợp đồng một cách tương tác, thực hiện một số động thái, hoàn tác một số động thái trong số đó và sau đó tiến hành theo một hướng khác.
- **Đặt lại** sẽ đặt lại hợp đồng và trạng thái của nó trở lại giá trị ban đầu: hợp đồng đầy đủ và trạng thái trống. Nó cũng *dừng* mô phỏng.

Trong ví dụ của chúng tôi, chúng ta hãy chọn Người *cho vay* thực hiện khoản tiền gửi 10.000 Ada. Chúng ta có thể làm điều đó bằng nút **+** bên cạnh đầu vào này. Sau khi làm điều đó chúng ta thấy:

![Bước mô phỏng 2](https://docs.marlowe.iohk.io/assets/images/simulation2-6039aaecca452ddebe7f5ed4e8da66ef.png)

nơi chúng tôi thấy ở bên phải màn hình rằng khoản tiền gửi đã được thực hiện, sau đó là thanh toán tự động cho Người *vay* . Chúng ta cũng có thể thấy rằng phần được đánh dấu đã thay đổi để phản ánh thực tế rằng khoản tiền gửi và thanh toán ban đầu đã được thực hiện.

Phần còn lại của hợp đồng là khoản hoàn trả: nếu chúng tôi chọn hành động này của Bên *vay* , chúng tôi thấy rằng hợp đồng đã hoàn thành.

![Bước mô phỏng 3](https://docs.marlowe.iohk.io/assets/images/simulation3-4570d13d5af5a5b26dfdf824e0f588b1.png)

Nhật ký ở phía bên phải màn hình hiện cung cấp danh sách đầy đủ các hành động được thực hiện bởi những người tham gia và chính hợp đồng. Một lưu ý cuối cùng: chúng tôi đã chọn không tạm ứng thời gian vào bất kỳ thời điểm nào: điều này phù hợp với thiết kế hợp đồng; mặt khác, chúng tôi không thấy bất kỳ hành động *hết thời gian* nào xảy ra. Tại sao không thử điều này cho mình?



## Oracle simulation

Như chúng tôi đã lưu ý trước đó trong phần về [Marlowe từng bước một](https://docs.marlowe.iohk.io/tutorials/concepts/marlowe-step-by-step) , Playground cung cấp các giá trị tiên tri cho các mô phỏng cho vai trò đó `"kraken"`. Khi mô phỏng đạt đến điểm mô phỏng cấu trúc này:

![Yêu cầu một giá trị tiên tri](https://docs.marlowe.iohk.io/assets/images/oracles1-aa312caa05aad2352adc217a72ad133b.png)

thì giá trị được *điền trước* vào mô phỏng như thế này:

![Cung cấp một giá trị oracle](https://docs.marlowe.iohk.io/assets/images/oracles2-e47d6804ac1e31489cf5a3aa5e3e62ba.png)



## Saving and Opening Projects

Các dự án có thể được lưu trên github và vì vậy khi bạn lưu dự án lần đầu tiên, bạn sẽ được nhắc như sau:

![Nhắc đăng nhập vào github](https://docs.marlowe.iohk.io/assets/images/github1-2aa55b659bc61cfe6ba80b32fb390850.png)

và, nếu bạn chọn **Đăng nhập** ở đó, bạn sẽ được đưa đến màn hình đăng nhập cho github:

![Đăng nhập vào github](https://docs.marlowe.iohk.io/assets/images/github2-16e38a9cb1754fabfc8ab1fd54042d3c.png)

Khi bạn chọn **Mở Dự án,** bạn sẽ thấy một lựa chọn như thế này:

![Lựa chọn dự án mở](https://docs.marlowe.iohk.io/assets/images/github3-5451043d1b996427344bca8088fd2a09.png)

Sân chơi Marlowe không cung cấp cơ chế xóa dự án, nhưng việc này có thể được thực hiện trực tiếp trên github.



## Analysing a contract

Phân tích tĩnh của hợp đồng được thực hiện bằng cách chọn tab **Phân tích tĩnh** ở chân trang ở cuối trang.

![Phân tích tĩnh](https://docs.marlowe.iohk.io/assets/images/static-analysis-286b327bf3285c579e3adf198f417872.png)

Để phân tích một *mẫu,* cần phải cung cấp giá trị cho bất kỳ tham số nào của mẫu đó, như bạn có thể thấy trong ảnh chụp màn hình.

Việc nhấp vào nút **Phân tích để cảnh báo** sẽ dẫn đến hợp đồng hiện tại *ở trạng thái hiện tại* đang được phân tích. Kết quả là có thể nói rằng hợp đồng đã vượt qua tất cả các cuộc kiểm tra hoặc giải thích nó thất bại như thế nào và đưa ra trình tự các giao dịch dẫn đến lỗi. Như một bài tập, hãy thử điều này với `Escrow`hợp đồng, thay đổi khoản tiền gửi ban đầu từ Alice thành một khoản nhỏ hơn 450 chiếc vòng tay. Thông tin chi tiết được đưa ra trong phần [phân tích tĩnh](https://docs.marlowe.iohk.io/tutorials/concepts/static-analysis) bên dưới.

Nút **Phân tích khả năng tiếp cận** sẽ kiểm tra xem có phần nào của hợp đồng sẽ không bao giờ được thực thi hay không, dù người tham gia tương tác với hợp đồng như thế nào.

Phân **tích hoàn tiền khi Đóng** sẽ kiểm tra xem liệu bất kỳ công `Close`trình xây dựng nào có thể hoàn trả tiền hay không hoặc liệu tất cả `Close`số tiền trong hợp đồng đã được hoàn trả hay chưa.

Sử dụng Marlowe Playground để tương tác với các hợp đồng mẫu và đặc biệt là thử các hợp đồng có các giá trị tham số khác nhau, đồng thời sửa đổi chúng theo nhiều cách khác nhau để xem các hợp đồng có thể không đáp ứng được phân tích như thế nào.
