# Singleton

**Design Pattern Singleton** giúp cho chúng ta đảm bảo rằng một class chỉ có một instance duy nhất và chúng được truy xuất thông qua một biến toàn cục.

### Problem:

- Chúng ta có một struct config gồm một biến cho biết được rằng nhiệm vụ đang xử lý có được cho phép log hay không.

- Trong hàm main, chúng ta có 1000 rps và đồng thời cũng mở ra 1000 goroutine để thực hiện xử lý. Trong hàm main này chúng ta có thể lấy ra được config vì chúng ta có khởi tạo NewConfig.

- Tuy nhiên cũng trong hàm main, chúng ta có hàm requestHandler, hàm requestHandler này lại không biết và không xử lý được config trên.

### Solution:

- Chúng ta sẽ cung cấp ra một biến toàn cục hoặc là một hàm toàn cục để trả về biến Singleton và cũng chỉ cần khởi tạo một lần duy nhất mà thôi.

- Và giờ quay lại với hàm requestHandler, chúng ta đã có thể gọi được biến Singleton trên để xử lý rồi sau đó quyết định có nên log hay không. Problem trên đã được xử lý, tuy nhiên ở phần còn lại của solution sẽ demo cho chúng ta thấy trường hợp bị Dataracing.

- Ví dụ chúng ta xét đến hàm loadConfig, sau khi mất ít thời gian để xử lý, cuối cùng hàm sẽ set config vào. Xết đến hàm GetConfig, chúng ta sử dụng if bình thường như bên dưới:

  ```golang
    if app.cfg == nil {
        log.Println("It should be run only once. But you'll it many times!!")
        app.loadConfig()
    }

    return app.cfg
  ```

- Đối với code trên, khi chạy chương trình chúng ta sẽ gặp tình trạng Dataracing như sau:

  ```plantext
  2024/05/24 14:27:11 Request 276 handled successfully.
  2024/05/24 14:27:11 Request 317 handled successfully.
  2024/05/24 14:27:11 Handling request 312... please wait.
  2024/05/24 14:27:11 Handling request 310... please wait.
  2024/05/24 It should be run only once. But you'll it many times!!
  2024/05/24 It should be run only once. But you'll it many times!!
  ```

- Chúng ta sẽ thấy cụ thể rằng đoạn <mark>It should be run only once. But you'll it many times!!</mark> sẽ xuất hiện nhiều lần. Chúng ta đang mong muốn nó chỉ chạy một lần, như vậy ta thấy chương trình đang bị Dataracing.

- Như vậy chúng ta sẽ cần cơ chế để lock nó lại, trong ngôn ngữ golang có thể dùng package sync, trong đó sẽ có once để thực hiện việc lock trên, và sau cùng ta sẽ áp dụng vào code:

  ```golang
    if app.cfg == nil {
        app.once.Do(func() {
  		log.Println("Loading config once and forever.")
  		app.loadConfig()
  	})
    }

    return app.cfg
  ```

### Summary:

- Điểm yếu của Singleton là nó khiến cho những chỗ đang dùng Singleton sẽ biết khá nhiều về thông tin nội dung biến. Vậy nên chúng ta nên dùng một interface để có thể trả về một interface giúp hạn chế bớt điều trên.
