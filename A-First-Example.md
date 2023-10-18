## A simple escrow contract

![](C:\Users\Admin\AppData\Roaming\marktext\images\2023-10-18-22-11-28-image.png)

Giả sử người đó `alice`muốn mua một con mèo từ `bob`, nhưng không ai trong số họ tin tưởng người kia. May mắn thay, họ có một người bạn chung `carol`mà cả hai đều tin tưởng là người trung lập (nhưng không đủ để đưa tiền cho cô ấy và làm trung gian). Do đó, họ đồng ý về hợp đồng sau, được viết bằng mã giả chức năng đơn giản. Loại hợp đồng này là một ví dụ đơn giản về *ký quỹ* .

```
When aliceChoice
     (When bobChoice
           (If (aliceChosen `ValueEQ` bobChosen)
               agreement
               arbitrate))
```

Nói chung `When`đưa ra *danh sách các trường hợp* , [mỗi](https://docs.marlowe.iohk.io/tutorials/concepts/escrow-ex#fn-1) trường hợp có một hành động và một hợp đồng tương ứng được kích hoạt khi hành động đó xảy ra. Bằng cách sử dụng điều này, chúng ta có thể cho phép tùy chọn `bob`đưa ra lựa chọn đầu tiên, thay vì `alice`, như thế này:

```
When [ Case aliceChoice
            (When [ Case bobChoice
                        (If (aliceChosen `ValueEQ` bobChosen)
                           agreement
                           arbitrate) ],
       Case bobChoice
            (When [ Case aliceChoice
                        (If (aliceChosen `ValueEQ` bobChosen)
                            agreement
                            arbitrate) ]
     ]
```

Trong hợp đồng này, Alice hoặc Bob có thể đưa ra lựa chọn đầu tiên; người kia sau đó đưa ra lựa chọn. Nếu họ đồng ý thì thế là xong; nếu không, Carol sẽ phân xử. Trong phần còn lại của hướng dẫn, chúng ta sẽ quay lại phiên bản đơn giản hơn ở phần `alice`được chọn trước tiên.



Hãy suy nghĩ về việc thực hiện hợp đồng này trong thực tế. Giả sử Alice đã cam kết một số tiền cho hợp đồng. Điều gì sẽ xảy ra nếu Bob chọn không tham gia nữa?

## Adding timeouts

Hợp đồng của Marlowe kết hợp các cấu trúc bổ sung để đảm bảo rằng chúng tiến triển đúng cách. Mỗi lần chúng tôi nhìn thấy `When`, chúng tôi cần cung cấp thêm hai thứ:

- thời *gian chờ* sau đó hợp đồng sẽ tiến triển và
- hợp đồng *tiếp tục* mà nó tiến tới.

```
When [ Case aliceChoice
            (When [ Case bobChoice
                        (If (aliceChosen `ValueEQ` bobChosen)
                           agreement
                           arbitrate) ]
                  1700007200    -- ADDED
                  arbitrate)    -- ADDED
      ]
      1700003600   -- ADDED
      Close        -- ADDED
```

Yêu cầu ngoài cùng `When`yêu cầu Alice đưa ra lựa chọn đầu tiên: nếu Alice không đưa ra lựa chọn trước thời gian POSIX `1700003600`(2023-11-14 23:13:20 GMT), hợp đồng sẽ bị đóng và tất cả số tiền trong hợp đồng sẽ được hoàn lại .

## Adding commitments

Tiếp theo, chúng ta nên xem cách *cam kết tiền mặt* là bước đầu tiên của hợp đồng.

```
When [Case (Deposit "alice" "alice" ada price)   -- ADDED
 (When [ Case aliceChoice
             (When [ Case bobChoice
                         (If (aliceChosen `ValueEQ` bobChosen)
                            agreement
                            arbitrate) ]
                   1700007200
                   arbitrate)
       ]
       1700003600
       Close)
   ]
   1700000000                              -- ADDED
   Close                                   -- ADDED
```

Một khoản tiền gửi `price`được yêu cầu từ `"alice"`: nếu nó được đưa ra, thì nó sẽ được giữ trong một tài khoản, còn được gọi là `"alice"`. Những tài khoản như thế này chỉ tồn tại trong thời hạn hợp đồng; mỗi tài khoản thuộc về một hợp đồng duy nhất.
