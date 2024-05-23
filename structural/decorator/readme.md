# Decorator

**Design Pattern Decorator** (Wrapper) giúp chúng ta có thể đính kèm những hành vi mới (behaviors) vào những Object bằng cách đặt chúng vào những wrapper đặc biệt mà có chứa các hành vi.

### Problem:

- Chúng ta có EmailNotifier và SMSNotifier hoán đổi được cho nhau thông qua interface Notifier và Notifier có hàm Send(message string). Vậy nếu như trong trường hợp mà service cần cả hai EmailNotifier và SMSNotifier, thì chúng ta sẽ cần tạo struct là EmailSMSNotifier mang theo cả hai notifier là EmailNotifier và SMSNotifier. Bản thân nó vẫn là một implement của Notifier nên chúng vẫn sẽ có hàm Send(message string), có điều là hàm Send này sẽ gọi vào emailNotifier và smsNotifier để thực hiện hàm Send.

- Như vậy nếu cần tăng thêm số lượng notifier thì chúng ta sẽ lập ra thêm (nhiều) những struct notifier để thực hiện các công việc của nó.

- Mặt khác chúng ta có thể giải quyết bằng cách tạo ra một notifier và sau đó sử dụng mảng của notifier rồi sau đó chúng ta thực hiện Send. Tuy nhiên khi sử dụng mảng, sẽ có sự hạn chế, không hiệu quả và không linh hoạt, thậm chí trong một số trường hợp không thể dùng mảng được, ví dụ như khi một hàm nào đó trả về Structure hay Object đơn lẻ thì sẽ không refactor cho nó trở thành một mảng được, điều đó có thể sẽ gây break cho những chỗ khác của chương trình.

- Tóm lại chúng ta đang gặp vấn đề liên quan tới cách để cấu trúc ra được class hay struct trong golang để nó có thể linh hoạt và hiệu quả cho công việc.

### Solution:

- Chúng ta vẫn có interface Notifier và có tới 3 struct EmailNotifier, SMSNotifier và TelegramNotifier.

- Tiếp theo chúng ta có NotifierDecorator, cấu trúc của NotifierDecorator sẽ có core và liên kết tới một decorator khác, và value sẽ là notifier. Có nghĩa là sẽ có một notifier nào đấy và bên trong có thể bọc một NotifierDecorator khác.

- Như vậy cấu trúc này sẽ giúp cho chúng ta có thể bọc (wrap) liên tục những decorator lại với nhau, đây chính là cấu trúc dữ liệu Stack.

- Tiếp theo chúng ta xét đến hàm Send(message string), chúng ta sẽ thấy đệ quy được sử dụng ở đây, nghĩa là hàm này sẽ gọi lại hàm Send và khác Object nhận vào.

- Để sử dụng hàm decorator trên chúng ta sẽ tạo hàm Decorate, truyền vào Notifier, và trả về NotifierDecorator. NotifierDecorator được trả về có core: &nd (chính là NotifierDecorator hiện tại), và có notifier chính là notifier mà ta truyền vào, ở đây chúng ta thấy được nó là Stack.

- Đến với cách sử dụng để decorator tại hàm main(), chúng ta có NewNotifierDecorator truyền EmailNotifier vào. Sau đó bổ sung lên thêm hai lớp là SMSNotifier và TelegramNotifier, ở đây sẽ sử dụng hàm mà Decorate mà được thiết kế cho decorator. Tiếp theo tại Service chúng ta SendNotification.

- Kết quả của chương trình như sau:

  ```bash
  Sending message: Hello world (Sender: Telegram)
  Sending message: Hello world (Sender: SMS)
  Sending message: Hello world (Sender: Email)
  ```

- Theo kết quả của chưởng trình ta thấy, Telegram sẽ được lấy và gửi trước, sau đó lần lượt là SMS rồi đến Email, đúng với cách hoạt động của Stack, nghĩa là Email vào trước, rồi đến SMS và Telegram, thì khi gửi ra sẽ theo thứ tự trước sau là Telegram, SMS, Email.

- Từ đó nếu như chúng ta muốn phối hợp nhiều loại notifier với nhau thì chúng ta chỉ cần thay đổi Decorate thôi là được, không cần phải tạo ra nhiều những class tượng trưng cho các notifier ở phối hợp nữa.

### Summary:

- Sau này chúng ta có thể sẽ có nhu cầu custom ra một error ở trong golang. Trong golang thì error sẽ là một interface, có nghĩa là bất kỳ một structure nào implement hàm error trả về string thì nó đều được gọi là error.

  ```golang
  func Wrap(err error, message string) error {
      if err == nil {
          return nil
      }
      err = &withMessage{
          cause: err,
          msg: message,
      }
      return &withStack{
          err,
          callers(),
      }
  }
  ```

- Vậy tại sao chúng ta lại cần phải wrap error theo kiểu decorator? Ví dụ chúng ta có một tầng data layer trả về lỗi, nhưng mà lỗi này thường sẽ khá nhạy cảm (sensitive) như là table không tìm thấy, câu query bị sai, và chúng ta không muốn trả lỗi như vậy về cho client, sẽ rất nguy hiểm. Vậy thì chúng ta sẽ bọc lỗi đó lại và trả về một message thân thiện hơn. đồng thời vẫn giữ lỗi gốc để khi cần vẫn sẽ print ra cho các maintainer biết được lỗi từ đâu.
