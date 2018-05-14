# TicTacToe - MVP

MVP phá vỡ các Controller thành view / activity mà không buộc nó với phần còn lại của các responsibility "controller".
Chi tiết hơn, nhưng chúng ta hãy bắt đầu lại với một định nghĩa chung về responsibility so với MVC.

## Model

Mô hình bao gồm Data + State + Business logic.

## View

Sự thay đổi duy nhất ở đây là các Activity / Fragment bây giờ được coi là một phần của View.
Chúng ta dừng lại việc chiến đấu với xu hướng tự nhiên để họ cùng đì.
Good practice là có Activity thực hiện một giao diện interface sao cho presenter có một giao diện để code.
Điều này loại bỏ khớp nối nó vào bất kỳ điểm cụ thể và cho phép unit test đơn giản với một implement mô hình của view.

## Presenter

Đây thực chất là controller từ MVC ngoại trừ việc nó không gắn liền với View, mà chỉ là một interface.
Việc này nêu lên những quan ngại khả năng kiểm thử cũng như các mối quan tâm của modularity/flexibility, chúng ta đã có với MVC.
*Trong thực tế, MVP chủ nghĩa thuần túy sẽ tranh luận rằng presenter không bao giờ nên có bất kỳ reference cho bất kỳ API Android hoặc code.*

View ([tictactoe.xml](https://github.com/nguyenkien25/TicTacToe-MVP/blob/master/app/src/main/res/layout/tictactoe.xml) [menu_tictactoe.xml](https://github.com/nguyenkien25/TicTacToe-MVP/blob/master/app/src/main/res/menu/menu_tictactoe.xml) [TicTacToeActivity](https://github.com/nguyenkien25/TicTacToe-MVP/blob/master/app/src/main/java/com/acme/tictactoe/view/TicTacToeActivity.java) [TicTacToeView <interface>](https://github.com/nguyenkien25/TicTacToe-MVP/blob/master/app/src/main/java/com/acme/tictactoe/view/TicTacToeView.java))
---Notify , Ask View to Setup Itself --> Presenter ([Presenter <Interface>](https://github.com/nguyenkien25/TicTacToe-MVP/blob/master/app/src/main/java/com/acme/tictactoe/presenter/Presenter.java) [TicTacToePresenter](https://github.com/nguyenkien25/TicTacToe-MVP/blob/master/app/src/main/java/com/acme/tictactoe/presenter/TicTacToePresenter.java))
---Interact with--> Model ([Board](https://github.com/nguyenkien25/TicTacToe-MVP/blob/master/app/src/main/java/com/acme/tictactoe/model/Board.java) [Cell](https://github.com/nguyenkien25/TicTacToe-MVP/blob/master/app/src/main/java/com/acme/tictactoe/model/Cell.java) [Player](https://github.com/nguyenkien25/TicTacToe-MVP/blob/master/app/src/main/java/com/acme/tictactoe/model/Player.java))

Nhìn vào Presenter chi tiết trong file [TicTacToePresenter](https://github.com/nguyenkien25/TicTacToe-MVP/blob/master/app/src/main/java/com/acme/tictactoe/presenter/TicTacToePresenter.java), điều đầu tiên bạn sẽ nhận thấy là cách đơn giản hơn nhiều và rõ ràng hơn trong intent của mỗi action.
Thay vì nói với view làm thế nào để hiển thị một cái gì đó, nó chỉ nói cho view những gì để hiển thị.

Để làm công việc này mà không buộc activity vào presenter, chúng ta tạo ra một interface mà các Activity implement.
Trong một test, chúng ta sẽ tạo ra một mô hình dựa trên interface này để kiểm tra sự tương tác với view từ những presenter.

## Evaluation

Đã clean hơn rất nhiều.
Chúng ta có thể dễ dàng unit test logic của presenter bởi vì nó không gắn với bất kỳ view cụ thể và Android API và đó cũng cho phép chúng ta làm việc với bất kỳ view khác miễn là view implement interface [TicTacToeView](https://github.com/nguyenkien25/TicTacToe-MVP/blob/master/app/src/main/java/com/acme/tictactoe/view/TicTacToeView.java).

## Presenter Concerns

- *Maintenance* - Presenters, giống như Controller, dễ bị sprinkled in, over time. Tại một số điểm, các developer thường xuyên tìm thấy chính mình với các presenter khó sử dụng lớn mà khó có thể phá vỡ.

## Và làm sao để sửa chúng? MVVM là cứu tinh!

Tất nhiên, các Developer cẩn thận có thể giúp ngăn chặn điều này, bởi siêng năng bảo vệ chống lại sự cám dỗ này là thay đổi ứng dụng theo thời gian.
Tuy nhiên, MVVM có thể giúp giải quyết điều này bằng cách làm ít để bắt đầu. 3. MVVM

MVVM với Data Binding trên Android có lợi ích của việc kiểm tra dễ dàng hơn và mô đun, trong khi cũng làm giảm số lượng mã glue mà chúng ta phải viết để kết nối các view + model.

Hãy kiểm tra các phần của MVVM.
