# Marlowe embedded in Haskell

(# Marlowe được nhúng trong Haskell)

Trong hướng dẫn này, chúng ta quay lại ví dụ về ký quỹ và chỉ ra cách chúng ta có thể sử dụng tính *năng nhúng* Marlowe trong Haskell để tạo ra các mô tả dễ đọc hơn, mô-đun hóa và có thể tái sử dụng hơn về các hợp đồng Marlowe.

![](C:\Users\Admin\AppData\Roaming\marktext\images\2023-10-18-23-36-49-image.png)

Hãy nhớ lại rằng chúng ta đã phát triển hợp đồng Marlowe này trong [hướng dẫn trước đó](https://docs.marlowe.iohk.io/tutorials/concepts/escrow-ex) .

Mặc dù chúng tôi trình bày nó ở đó dưới dạng hợp đồng "nguyên khối", nhưng chúng tôi có thể sử dụng các định nghĩa Haskell để làm cho nó dễ đọc hơn. Để bắt đầu, chúng ta có thể tách cam kết ban đầu khỏi phần hoạt động *bên trong* của hợp đồng:

```
contract :: Contract
contract = When [Case (Deposit "alice" "alice" ada price) inner]
                1700000000
                Close

inner :: Contract
inner =
  When [ Case aliceChoice
              (When [ Case bobChoice
                          (If (aliceChosen `ValueEQ` bobChosen)
                              agreement
                              arbitrate) ]
                    1700007200
                    arbitrate)
       ]
       1700003600
       Close
```

Nhiều thuật ngữ ở đây được định nghĩa trong Haskell. Về cơ bản, chúng ta có hai hợp đồng giải quyết những gì xảy ra `agreement`giữa Alice và Bob, và nếu không thì Carol nên làm gì `arbitrate`giữa họ:

```
agreement :: Contract
agreement =
  If (aliceChosen `ValueEQ` (Constant 0))
     (Pay "alice" (Party "bob") ada price Close)
     Close

arbitrate :: Contract
arbitrate =
  When [ Case carolClose Close,
         Case carolPay (Pay "alice" (Party "bob") ada price Close) ]
       1700010800
       Close
```

Trong các hợp đồng này, chúng tôi cũng sử dụng các từ viết tắt đơn giản như:

```
price :: Value
price = Constant 450
```

cho biết giá của con mèo và giá trị của số tiền được ký quỹ.

Chúng ta cũng có thể mô tả các lựa chọn do Alice và Bob đưa ra, lưu ý rằng chúng ta cũng được yêu cầu một giá trị mặc định đề `defValue`phòng trường hợp các lựa chọn đó chưa được thực hiện.

```
aliceChosen, bobChosen :: Value

aliceChosen = ChoiceValue (ChoiceId choiceName "alice")
bobChosen   = ChoiceValue (ChoiceId choiceName "bob")

defValue = Constant 42

choiceName :: ChoiceName
choiceName = "choice"
```

Khi mô tả các lựa chọn, chúng ta có thể đặt tên hợp lý cho các giá trị số:

```
pay,refund,both :: [Bound]

pay    = [Bound 0 0]
refund = [Bound 1 1]
both   = [Bound 0 1]
```

và xác định *các chức năng* mới (hoặc "mẫu") cho chính chúng ta. Trong trường hợp này chúng tôi xác định

```
choice :: Party -> [Bound] -> Action

choice party bounds =
  Choice (ChoiceId choiceName party) bounds
```

như một cách làm cho việc thể hiện các lựa chọn trở nên đơn giản hơn và dễ đọc hơn:

```
alicePay, aliceRefund, aliceChoice :: Action
alicePay    = choice "alice" pay
aliceRefund = choice "alice" refund
aliceChoice = choice "alice" both
```

Với tất cả các định nghĩa này, chúng ta có thể viết hợp đồng ở đầu phần này theo cách làm rõ ý định của nó. Viết bằng Marlowe "thuần túy", hoặc bằng cách mở rộng các định nghĩa này, thay vào đó chúng ta sẽ có hợp đồng này:

```
When [
  (Case
     (Deposit
        "alice" "alice" ada
        (Constant 450))
     (When [
           (Case
              (Choice
                 (ChoiceId "choice" "alice") [
                 (Bound 0 1)])
              (When [
                 (Case
                    (Choice
                       (ChoiceId "choice" "bob") [
                       (Bound 0 1)])
                    (If
                       (ValueEQ
                          (ChoiceValue
                             (ChoiceId "choice" "alice"))
                          (ChoiceValue
                             (ChoiceId "choice" "bob")))
                       (If
                          (ValueEQ
                             (ChoiceValue
                                (ChoiceId "choice" "alice"))
                             (Constant 0))
                          (Pay
                             "alice"
                             (Party "bob") ada
                             (Constant 450) Close) Close)
                       (When [
                             (Case
                                (Choice
                                   (ChoiceId "choice" "carol") [
                                   (Bound 1 1)]) Close)
                             ,
                             (Case
                                (Choice
                                   (ChoiceId "choice" "carol") [
                                   (Bound 0 0)])
                                (Pay
                                   "alice"
                                   (Party "bob") ada
                                   (Constant 450) Close))] 100 Close)))] 60
                 (When [
                       (Case
                          (Choice
                             (ChoiceId "choice" "carol") [
                             (Bound 1 1)]) Close)
                       ,
                       (Case
                          (Choice
                             (ChoiceId "choice" "carol") [
                             (Bound 0 0)])
                          (Pay
                             "alice"
                             (Party "bob") ada
                             (Constant 450) Close))] 100 Close)))
      ]
```


