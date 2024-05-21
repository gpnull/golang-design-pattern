# Abstract Factory

**Design Pattern Abstract Factory** giúp khởi tạo ra được những Object tương đồng với nhau mà không cần phải chỉ định ra class cụ thể của nó.

### Problem:

- Chúng ta có Voucher là gồm có đồ uống và đồ ăn, chúng đều là interface, đồ uống này sẽ có hàm là Drink() và đồ ăn thì sẽ có hàm là Eat().

- Như vậy theo problem, Coffee sẽ là đồ uống, Beer sẽ là đồ uống, Cake sẽ là đồ ăn và GrilledOctopus sẽ là đồ ăn.

- Từ đó chúng ta sẽ có thể thiết lập ra được danh sách các Voucher như sau:

  - Đầu tiên chúng ta có: Coffee và Cake, rất ổn.
  - Thứ hai sẽ là Beer và GrilledOctopus, cũng rất tuyệt vời.
  - Cuối cùng đó là Coffee và GrilledOctopus, có vẻ bất ổn rồi và User sẽ không chắc là họ sẽ dùng được Voucher này.

- Về kỹ thuật về lập trình thì nó sẽ không có lỗi. Nhưng mà nếu mà nói về business logic thì chúng ta nhận thấy đây là vấn đề cần giải quyết.

- Như vậy chúng ta sẽ cần một phương thức nào đó để tạo ra được những Voucher này tốt hơn. Từ đó chúng ta sẽ áp dụng Abstract Factory

### Solution:

- Chúng ta sẽ khởi tạo interface VoucherAbstractFactory trả về phương thức GetDrink() và GetFood(). Tiếp tục chúng ta sẽ có hai struct là CoffeeMorningVoucherFactory (dùng cho phục vụ bữa sáng) và DrinkEveningVoucherFactory (dùng cho phục vụ bữa tối).

- Chúng ta sẽ dùng Factory Method là GetVoucherFactory truyền vào campaignName (creative-morning, chill-all-night-long, ...).

- Và từ một Abstract Factory thì chúng ta có thể lấy ra được Voucher rất đơn giản, chúng ta chỉ cần return Voucher ra với Drink và Food được lấy từ factory của VoucherAbstractFactory là xong.

- Như vậy trong hàm main chúng ta sẽ lấy được voucherFactory bằng cách sử dụng GetVoucherFactory để truyền campaignName vào. Đứng từ voucherFactory này chúng ta sẽ lấy ra được Voucher mong muốn của chúng ta. Như vậy cuối cùng chúng ta sẽ có được những Voucher hợp lý hơn.

### Summary:

- Chúng ta thường rất dễ bị nhầm lẫn Abstract Factory và Method Factory.

- Nếu chúng ta viết ra một method hoặc là một structure nào đó mà để nó create thẳng ra luôn Voucher cho chúng ta, thì đó là một Factory Method.

- Còn ở ví dụ trên chúng ta phải dùng abstract, có nghĩa là một Factory là một interface cung cấp ra những thành phần cho Voucher đó cần, thì nó sẽ là abstract.

- Abstract Factory thường hay đi chung với Method Factory giống như trường hợp ví dụ trên.
