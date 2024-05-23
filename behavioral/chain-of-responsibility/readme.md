# Chain of Responsibility

**Design Pattern Chain of Responsibility** (CoR, Chain of Command) giúp chúng ta có thể pass được những request vào cùng với chuỗi các handler. Và dựa trên các request này thì mỗi handler sẽ quyết định hoặc là xử lý nó hoặc là sẽ pass nó tới handler tiếp theo trong chuỗi.

### Problem:

- Chúng ta có WebCrawler, WebCrawler này sẽ đi lấy nội dung của trang web bất kỳ thông qua đường link truyển vào. Bên trong hàm Crawl có bốn bước cơ bản, thứ nhất là đi check url hợp lê không, thứ hai là kéo nội dung đó về như là html, thứ ba là extract những thông tin từ html ra thành data, và cuối cùng sẽ lưu trữ data đó vào database.

- Thay vì chúng ta viết code hết trong hàm Crawl, thì chúng ta có thể chia thành nhiều step (responsibility) để xử lý. Đây cũng là mindset rất quan trọng khi chúng ta làm việc với hệ thống phân tán (microservice) về sau, các service có thể giao tiếp với nhau thông qua cơ chế, event nào đó.

### Solution:

- Chúng ta có struct Context, là parameter sẽ đi xuyên suốt qua tất cả những handler của code. Tiếp theo có một handler là function truyền vào con trỏ Context và trả về error.

- Tiếp theo phân tách những service ở problem trên thành nhiều hàm khác nhau như CheckingUrlHandler, FetchContentHandler, ExtractDataHandler, SaveDataHandler. Ở một số hàm handler như CheckingUrlHandler sau khi xử lý thì có thể return nil hoặc error, một số hàm handler khác như FetchContentHandler thì khi xử lý xong, dữ liệu sẽ được lưu vào Context để những handler khác có dữ liệu đó mà tiếp tục xử lý.

- Để sử dụng chúng ta sẽ có struct HandlerNode, gồm hàm hdl Handler và next trỏ đến handler tiếp theo cần xử lý là handler nào.

- Đến với hàm Handle, nếu node không lỗi thì hàm sẽ thực thi các hdl thông qua việc truyền vào context vào, sau khi thực thi, nếu có lỗi hàm sẽ trả về nil và không làm gì tiếp, còn nếu thực thi thành công, hàm sẽ return ra node tiếp theo.

- Tiếp đó là hàm NewCrawler khởi tạo ra một chuỗi danh sách nhiệm vụ, truyền vào n handlers. Trong hàm sẽ duyệt và tạo ra danh sách các hdl.

- Cuối cùng đến hàm main có chứa WebCrawler truyền vào handler là một danh sách các handler như CheckingUrlHandler, FetchContentHandler, ExtractDataHandler, SaveDataHandler. Chạy chương trình thu được kết quả:

  ```bash
  Checking url: https://some-rich-content-website/some-page
  Fetching content from url: https://some-rich-content-website/some-page
  Extracting data from content:
  Saving data to database: %!s(<nil>)
  ```

- Kết quả cho thấy chương trình chạy đúng theo những thứ tự handler mà chúng ta đã thiết lập.

### Summary:

- Ưu điểm của Design Pattern Chain of Responsibility sẽ giúp chúng ta có thể quản lý được những handler, chúng sẽ có nhiệm vụ duy nhất được quy định, chúng sẽ phối hợp được với nhau, chúng sẽ được dùng lại.

- Chain of Responsibility rất quen thuộc, có thể chúng ta chưa bao giờ thiết lập nó, nhưng khả năng cao chúng ta đã sử dụng nó rồi.

- Ví dụ trong Backend chúng ta có request truy vấn tới một handler nào đó, trước nó những thứ như là authen, authorize, log,... thì chúng ta sẽ thường dùng tới middleware. Middleware cũng thường được ghép nối từ nhiều service tạo thành một chuỗi service để xử lý một nhiệm vụ cụ thể nào đó, nếu nhiệm vụ đó bị lỗi trong quá trình chạy của middleware, nó sẽ bị return. Như vậy thì middleware là một trong những implementation của Chain of Responsibility.
