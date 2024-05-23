# Factory Method

**Design Pattern Factory Method** (Virtual Constructor) cung cấp interface cho việc khởi tạo ra các Object trong cùng một superclass. Tuy nhiên, nó sẽ cho phép các subclasses này có thể thay đổi được type của những Object trong lúc được tạo ra.

### Problem:

- Tại hàm main, nơi tạo ra một service thì chúng ta có thể truyền bất kỳ một notify nào vào đây (vd: EmailNotifier{}) miễn là nó implement Notifier.
- Tuy nhiên trên thực tế, chúng ta sẽ muốn là phải thông qua một hàm nào đó có thể trả về được notifier ví dụ như là CreateNotifier(type) và trả về notifier.

### Solution:

- Giải pháp chúng ta là sẽ tạo một hàm CreateNotifier, trả về interface Notifier.
- Trên thực tế Factory Method chúng ta có thể tạo từng structure để khởi tạo ra đúng loại notifier cũng được.

### Summary:

- Như vậy chúng ta cần phải sử dụng các method để khởi tạo ra các Object đúng type cho việc return, cho nên nó mới có cái tên khác là Virtual Constructor (hàm khởi tạo).
