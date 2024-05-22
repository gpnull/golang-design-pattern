# Composite

**Design Pattern Composite** giúp chúng ta có thể tạo ra được những Object dưới dạng cấu trúc cây và sau đó có thể làm việc với những cấu trúc này như thể nó là những Object.

### Problem:

- Chúng ta có struct Item gồm có Name, Price và children là mảng Item. Chúng ta sẽ thường gặp vấn đề này ở những trường hợp như cây danh mục, ban đầu chỉ có một cấp, nhưng sau đó sẽ phân thành nhiều cấp thì chúng ta sẽ đưa vào trong mảng. Vậy điều chúng ta thấy ở problem này là Item này sẽ có item con của nó.

- Xem tiếp problem, chúng ta có hàm Cost để tính giá đơn giản, chúng ta lấy giá của chính nó và cộng dồn cho những item của nó vào nữa, đến cuối cùng chúng ta sẽ ra được cost cuối cùng.

- Chúng ta có một hàm CreatePackage đóng gói một item, ta thấy item này không nằm riêng lẻ mà lại có nhiều cấp. Ví dụ ban đầu nó sẽ nằm trong hộp có Name: "root box", Price: 0, thì ngay từ đầu chúng ta đã thấy sự bất ổn rồi, đó là ban đầu chỉ là cái hộp (container) thôi mà nó cũng phải chứa Name và Price.

- Tiếp theo sau đó là children: []Item là một mảng chứa item đúng nghĩa gồm Name và Price, children không có thì có giá trị nil. Tiếp tục chúng ta lại có một hộp khác có Name: "sub box" nằm trong hộp có Name: "root box" ban đầu, và tiếp tục lặp lại vấn đề của hộp có Name: "root box" bên trên.

- Từ đây có thể dễ hình dung ra được đây là một cấu trúc cây, nhưng trong đó nó lại có những thành phần không được hợp lý.

### Solution:

- Chúng ta có một Item là một interface có hàm Cost bởi vì như problem trên thì chúng ta đang quan tâm tới việc tính giá mà thôi. Từ đó, RealItem có Name và Price, chúng ta sẽ thấy là không còn children nữa.

- Tiếp theo chúng ta tạo một struct Box, Box này sẽ không có Name và Price nữa, mà nó sẽ có một mảng children chứa các Item. Bản thân của Box này cũng có hàm Cost có nhiệm vụ cộng dồn giá của các children trong nó và về cost.

- Cuối cùng chúng ta quay về hàm CreatePackage, vẫn trả về một Item, Item ở đây return về một Box bọc ngoài cùng, trong nó có children là một mảng Item chứa RealItem và một Box khác.

- Với cách khai báo trên chúng ta thấy Box bây giờ không còn bị đặt Name và Price không cần thiết vào nữa. nhìn hợp lý hơn và cũng dễ hiểu hơn.

- Có thể chúng ta sẽ đặt câu hỏi là nếu như có một Box mà cần có giá thì sao? Vậy thì chúng ta có thể tạo ra một struct BoxItem, BoxItem này bọc lấy RealItem và kèm theo trong đó sẽ có mảng children là vấn đề sẽ được giải quyết. Khi đó thì cả Box, RealItem và BoxItem đều được implement từ interface Item.
