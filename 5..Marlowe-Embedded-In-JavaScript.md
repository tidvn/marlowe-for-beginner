# Marlowe Embedded In JavaScript

(Marlowe được nhúng trong JavaScript)

Marlowe được viết dưới dạng kiểu dữ liệu Haskell và do đó, việc mô tả các hợp đồng thông minh của Marlowe bằng Haskell rất đơn giản. Nhưng vì hợp đồng Marlowe "chỉ" là một dạng dữ liệu nên chúng tôi có thể trình bày chúng tốt như nhau bằng các ngôn ngữ khác hỗ trợ tuần tự hóa thành JSON hoặc CBOR.

Ở đây chúng tôi mô tả một thư viện được viết bằng TypeScript có thể được sử dụng để tạo hợp đồng thông minh Marlowe từ TypeScript hoặc JavaScript theo cách tương tự như cách sử dụng Haskell. Nếu bạn không quen với TypeScript, bạn cũng có thể sử dụng API như thể nó được viết bằng JavaScript vì TypeScript là siêu bộ JavaScript.

## Using the JS Editor in the Marlowe Playground

(Sử dụng Trình soạn thảo JS trong Marlowe Playground)

Bản thân việc triển khai thư viện rất đơn giản và bạn có thể tìm thấy tất cả mã nguồn của nó tại đây:

- https://github.com/input-output-hk/marlowe-playground/blob/main/marlowe-playground-client/src/Language/Javascript/MarloweJS.ts

Nó dựa trên nguyên tắc rằng đối với mỗi *loại* Haskell sẽ có một loại TypeScript tương ứng và tương ứng với mỗi *hàm tạo* sẽ có một định nghĩa không đổi.

```
import {
   Address, Role, Account, Party, ada, AvailableMoney, Constant, ConstantParam,
   NegValue, AddValue, SubValue, MulValue, DivValue, ChoiceValue, TimeIntervalStart,
   TimeIntervalEnd, UseValue, Cond, AndObs, OrObs, NotObs, ChoseSomething,
   ValueGE, ValueGT, ValueLT, ValueLE, ValueEQ, TrueObs, FalseObs, Deposit,
   Choice, Notify, Close, Pay, If, When, Let, Assert, SomeNumber, AccountId,
   ChoiceId, Token, ValueId, Value, EValue, Observation, Bound, Action, Payee,
   Case, Timeout, ETimeout, TimeParam, Contract
} from 'marlowe-js';
```

Thư viện JavaScript/TypeScript cung cấp các định nghĩa không đổi cho các cấu trúc Marlowe không có đối số, như trường hợp của `Close`hợp đồng:

```
const Close: Contract
```

Các cấu trúc có đối số được biểu diễn dưới dạng hàm, như trong trường hợp `AvailableMoney`:

```
const AvailableMoney: (token: Token, accountId: Party) => Value
```

Hoặc các hàm và hằng số của thư viện JavaScript/TypeScript trả về biểu diễn JSON của các cấu trúc Marlowe. Ví dụ: hàm `AvailableMoney`được định nghĩa như sau:

```
const AvailableMoney =
    function (token: Token, accountId: Party) => Value {
        return { "amount_of_token": token,
                 "in_account": accountId };
    };
```

## The `SomeNumber` type

Có một loại quan trọng không có trong định nghĩa Haskell của Marlowe, chúng tôi gọi loại đó là SomeNumber và nó được định nghĩa như sau:

```
type SomeNumber = string | number | bigint
```

Lý do chúng tôi có loại này là vì loại gốc cho số trong JavaScript và TypeScript mất độ chính xác khi sử dụng với số nguyên lớn. Điều này là do việc triển khai nó phụ thuộc vào số dấu phẩy động.

Biểu thức sau đây đúng trong JavaScript:

```
9007199254740992 == 9007199254740993
```

Điều này có thể gây rắc rối cho các hợp đồng tài chính vì cuối cùng nó có thể dẫn đến thua lỗ.

Do đó, chúng tôi khuyên bạn nên sử dụng `bigint`loại. Nhưng chúng tôi hỗ trợ ba cách biểu diễn số để thuận tiện và tương thích ngược với các phiên bản cũ của JS:

- Số gốc:
  - Chúng rất dễ sử dụng
  - Ký hiệu rất đơn giản và có thể được sử dụng với các toán tử tiêu chuẩn, ví dụ:`32 + 57`
  - Họ mất độ chính xác với số lượng lớn
- Biểu diễn chuỗi:
  - Ký hiệu chỉ yêu cầu thêm dấu ngoặc kép xung quanh các con số
  - Bạn không thể sử dụng trực tiếp các toán tử tiêu chuẩn, ví dụ: `"32" + "57" = "3257"`
  - Họ không mất độ chính xác
- `bigint`kiểu:
  - Chúng rất dễ sử dụng (chỉ cần thêm `n`chữ sau số)
  - Ký hiệu rất đơn giản và có thể được sử dụng với các toán tử tiêu chuẩn, ví dụ:`32n + 57n`
  - Họ không mất độ chính xác

Tất cả các cách biểu diễn này đều được chuyển đổi sang `BigNumber`nội bộ, nhưng việc mất độ chính xác có thể xảy ra nếu số gốc được sử dụng, như được `BigNumber`xây dựng, trước khi chuyển đổi xảy ra và API không thể thực hiện bất kỳ điều gì về việc đó.



## The `EValue` type and boolean overloading

Trong Haskell, các quan sát boolean hằng số được biểu thị bằng `TrueObs` và `FalseObs`và các giá trị nguyên không đổi được biểu thị `Constant`bằng dấu `Integer`. Trong JavaScript và TypeScript, bạn cũng có thể sử dụng các hàm tạo này, nhưng bạn không cần phải làm vậy, vì loại Quan sát bị quá tải để chấp nhận các boolean JavaScript gốc và các hàm trong Haskell lấy a , thay vào đó trong JavaScript chúng lấy `Value`a `EValue`và `EValue`là được xác định như sau:

```
type EValue = SomeNumber | Value
```



## Example: Writing a Swap contract in TypeScript

Giả sử chúng ta muốn viết một hợp đồng để Alice có thể đổi 1000 Ada với Bob lấy 100 USD.

Trước tiên, hãy tính số lượng chúng ta muốn làm việc với từng đơn vị, chúng ta có thể xác định một số hằng số bằng cách sử dụng `const`:

```
const lovelacePerAda : SomeNumber = 1000000n;
const amountOfAda : SomeNumber = 1000n;
const amountOfLovelace : SomeNumber = lovelacePerAda * amountOfAda;
const amountOfDollars : SomeNumber = 100n;
```

Số tiền trong hợp đồng phải được viết bằng Lovelace, là 0,000001 Ada. Vì vậy chúng ta tính số lượng Lovelace bằng cách nhân 1.000 Ada cho 1.000.000. Số tiền đô la trong ví dụ của chúng tôi là 100.

API đã cung cấp hàm tạo cho tiền tệ ADA và hiện tại không có ký hiệu tiền tệ trong Cardano cho đô la, nhưng chúng ta hãy tưởng tượng là có và hãy xác định nó như sau:

```
const dollars : Token = Token("85bb65", "dollar")
```

Trên thực tế, chuỗi này `"85bb65"`sẽ tương ứng với ký hiệu tiền tệ, là một hàm băm và phải được viết bằng cơ số 16 (biểu diễn thập lục phân của chuỗi byte). Và chuỗi `"dollar"`sẽ tương ứng với tên mã thông báo.

Bây giờ chúng ta hãy xác định một loại đối tượng để chứa thông tin về các bên và những gì họ muốn trao đổi để thuận tiện:

```
type SwapParty = {
 party: Party;
 currency: Token;
 amount: SomeNumber;
};
```

Chúng ta sẽ lưu tên của bên đó vào trường bên, tên loại tiền tệ ở trường tiền tệ và số lượng tiền tệ mà bên đó muốn đổi ở trường số tiền:

```
const alice : SwapParty = {
   party: Role("alice"),
   currency: ada,
   amount: amountOfLovelace
}

const bob : SwapParty = {
   party: Role("bob"),
   currency: dollars,
   amount: amountOfDollars
}
```

Bây giờ chúng ta đã sẵn sàng để bắt đầu viết hợp đồng. Đầu tiên hãy xác định tiền gửi. Chúng tôi lấy thông tin từ bên phải gửi tiền, thời gian chờ đợi mà chúng tôi sẽ đợi khoản tiền gửi được thực hiện và hợp đồng tiếp tục sẽ được thực thi nếu khoản tiền gửi thành công.

```
function makeDeposit(src: SwapParty, timeout: ETimeout,
                     timeoutContinuation: Contract, continuation: Contract): Contract {
    return When([Case(Deposit(src.party, src.party, src.currency, src.amount),
                      continuation)],
                timeout,
                timeoutContinuation);
}
```

Chúng tôi chỉ cần một `When`cấu trúc có một đơn vị `Case`đại diện cho một `Deposit`bên `src`trong tài khoản của chính họ, theo cách này nếu chúng tôi hủy bỏ hợp đồng trước khi hoán đổi thì mỗi bên sẽ lấy lại được số tiền họ đã gửi.

Tiếp theo chúng ta xác định một trong hai khoản thanh toán của hợp đồng hoán đổi. Chúng tôi lấy các bên nguồn và đích làm tham số, cũng như hợp đồng tiếp tục sẽ được thực thi sau khi thanh toán.

```
const makePayment = function (src: SwapParty, dest: SwapParty,
                              continuation: Contract): Contract {
    return Pay(src.party, Party(dest.party), src.currency, src.amount,
               continuation);
}
```

Đối với điều này, chúng ta chỉ cần sử dụng `Pay`cấu trúc để thanh toán từ tài khoản mà bên nguồn đã gửi tiền cho bên đích.

Cuối cùng chúng ta có thể kết hợp tất cả các phần:

```
const contract: Contract = makeDeposit(alice, 1700000000n, Close,
                             makeDeposit(bob, 1700003600n, Close,
                                 makePayment(alice, bob,
                                     makePayment(bob, alice,
                                         Close))))

return contract;
```
