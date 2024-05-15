# Strategy

**Design Pattern Strategy** giúp định nghĩa ra được những thuật toán mà nó tương đồng với nhau đặt chúng vào những Object riêng biệt và chúng có thể hoán đổi được cho nhau.

### Problem:

- Một service gửi Notification, hàm gửi Notification sẽ được phân biệt như sau: với type Email thì chúng ta sẽ làm với Email, với type là SMS thì chúng ta sẽ làm với SMS.
- Như vậy, trong tương lai nếu như chúng ta có những type khác thì chúng ta sẽ tăng if/else để chúng ta làm những công việc đó, dẫn đến chúng ta cứ phải sửa hàm này.
- Suy ra, thứ nhất là chúng ta đang vi phạm nguyên lý O (Open/closed principle) và chúng ta cũng vi phạm nguyên lý S (Single responsibility principle) luôn bởi vì hàm đang làm quá nhiều trách nhiệm thay vì chỉ làm đúng business của nó.

### Solution:

- Chúng ta sẽ cần khai báo một interface Notifier có hàm Send, khai báo các struct EmailNotifier và SMSNotifier, và cả 2 sẽ cùng implement interface Notifier.
- Trong golang, chúng ta chỉ cần có method Send là sẽ được tự động implement trong hàm rồi. Từ đó, trong service NotificationService chúng ta chỉ cần đơn giản là viết notifier thuộc kiểu interface Notifier. Dẫn đến hàm SendNotification chúng ta không cần if/else nữa mà chúng ta chỉ cần gọi đến hàm Send của notifier.
- Từ đó có thể thấy rằng cho dù có tăng thêm, ví dụ như chúng ta tăng thêm một struct TelegramNotifier, thì chúng ta cũng chỉ là tăng thêm implement nữa mà thôi.
- Hàm SendNotification của chúng ta bây giờ nó không cần phải thay đổi nữa, và nó chỉ cần làm đúng business logic của chính nó.
- Cuối cùng là cách chạy chương trình, chúng ta sẽ có một service truyền vào Email hoặc là SMS miễn là nó đang implement interface Notifier, sau đó chúng ta sẽ gọi tới SendNotification từ service là xong.

### Summary:

- Như vậy sử dụng Strategy sẽ giúp cho ứng dụng được các nguyên lý S nghĩa là chúng sẽ làm đúng nhiệm vụ của nó mà thôi, và khi mà muốn tăng thêm số lượng Notifier thì chỉ có tăng thêm implement mà thôi, chúng ta sẽ viết thêm một class mới, một structure mới, dẫn đến chúng ta cũng đang tuân thủ nguyên lý O.
- Tuy nhiên thì Strategy cũng có một số điểm cần lưu ý. Đầu tiên là sẽ tăng tính phức tạp của chương trình lên, đương nhiên rồi vì nếu nó giúp cho chúng ta đảm bảo được những nguyên lý S và O thì thường nó sẽ tăng tính phức tạp lên. Thứ hai là nếu như số lượng Notifier của Chúng ta gần như sẽ không thay đổi theo thời gian thì chúng ta không cần phải dùng gì làm gì.
