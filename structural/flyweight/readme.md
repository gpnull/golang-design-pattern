# Flyweight

**Design Pattern Flyweight** (Cache) giúp chúng ta đưa những Object vào vùng memory và chia sẻ lại với nhau cho những Object khác thay vì phải chứa lại hết toàn bộ dung lượng cho Object này.

### Problem:

- Chúng ta sẽ có một phòng chat phòng chat và rất nhiều câu chat ở trong đó. Cấu trúc của một câu chat cơ bản chúng ta sẽ có Content, SenderName và SenderAvatar.
- Chúng ta có thể đảm bảo một điều là số lượng người trong phòng chat thường sẽ ít hơn số lượng câu chat rất nhiều và những đối tượng Sender này là lặp đi lặp lại mà thôi.
- Ví dụ chúng ta có Peter và Mary, họ sẽ có avatar tương ứng, của Peter là khoảng 300 kb và của Mary là khoảng 400 kb.

- Từ đó nếu chúng ta cứ đi lặp đi lặp lại toàn bộ thông tin của Sender, thì tổng số lượng tin nhắn sẽ có dung lượng cho Avatar đó là 1.4 MB hoặc cao hơn.

- Suy ra chúng ta cần cấu trúc lại để giúp tối ưu được lượng memory này. Có nghĩa là những vùng memory cho Avatar này nếu mà có rồi thì chúng ta sẽ dùng lại chứ chúng ta sẽ không đi tạo ra nữa (caching).

### Solution:

- Chúng ta sẽ tạo một structure ChatMessage gồm có Content là string và Sender sẽ giữ con trỏ đến Sender. Sender là struct gồm có Name và Avatar. Tiếp theo chúng ta tạo một struct SenderFactory để lấy ra được sender, lưu dưới dạng map có key là string (tên của sender) và value chính là con trỏ của Sender

- Từ đó chúng ta sẽ có senderFactory được tạo ra với hai người là Peter là Mary, trong ChatMessage bên dưới thì chúng ta chỉ còn lại Content và những Sender được lấy từ các SenderFactory.

- Từ đó tổng dung lượng cho các Avatar chỉ còn lại 700 kb chứ không duplicate ra với những tin nhắn mới.

### Summary:

- Flyweight là một design pattern mà chắc chắn chúng ta đã và đang làm, có thể chúng ta sẽ không nhận thấy rõ là vì có khả năng chúng ta đang dùng một thư viện nào đó đã implement cho chúng ta rồi. Thông thường trong thư viện sẽ implement cho chúng ta chỗ caching và nó sẽ có một cơ chế quản lý hiệu quả hơn.
