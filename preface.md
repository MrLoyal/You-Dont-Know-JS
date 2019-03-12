# Bạn đếch biết JS 
# Lời tựa

Chắc bạn đã nhận ra "JS" trong tựa đề của bộ sách này không phải là viết tắt của cụm từ mà người ta hay dùng để chửi rủa về JavaScript, mặc dù chửi rủa những điều kì quặc của ngôn ngữ là điều mà tất cả chúng ta có thể tìm thấy sự đồng cảm với nhau trong đó.

Từ những ngày đầu của web, JavaScript đã là công nghệ nền tảng trong việc tạo ra các trải nghiệm tương tác xung quanh những nội dung mà chúng ta tiêu thụ. Nếu ngày đầu, hiệu ứng lung linh theo sau con trỏ chuột, hay hiển thị hộp thoại để nhập thông tin là những ứng dụng khởi đầu của JavaScript, thì sau gần 2 thập kỷ, công nghệ và khả năng của JavaScript đã phát triển lên một tầm mới, chẳng còn nghi ngờ gì nữa, nó đã trở thành phần quan trọng trong phần lõi của nền tảng phần mềm phổ biến nhất thế giới: đó là web.

Nhưng là một ngôn ngữ, nó luôn luôn là một mục tiêu cho rất nhiều sự chỉ trích, một phần là do sự kế thừa, nhưng nguyên nhân lớn hơn nằm ở triết lý thiết kế của nó. Như Brendan Eich cũng đã từng nói, ngay cả cái tên cũng gợi lên "một thằng em ngốc" khi đặt cạnh người anh Java già dặn và trưởng thành hơn. Nhưng cái tên chỉ là một tai nạn trong lĩnh vực chính trị và marketing. Hai ngôn ngữ lập trình này hầu hết khác hẳn nhau trong rất nhiều khía cạnh. JavaScript và Java có mối liên hệ với nhau giống như "Nhật" và "Nhật Tân" vậy.

Bởi vì JavaScript có vay mượn ý tưởng và cú pháp từ các ngôn ngữ khác, bao gồm có thủ tục của ngôn ngữ C đầy tự hào, hay kiểu hàm đầy tinh tế của Scheme/Lisp. Nó cực kỳ dễ tiếp cận cho phần lớn các nhà phát triển, kể cả với những người có rất ít hoặc chưa hề có kinh nghiệm. Chương trình "Hello world" của JavaScript quá dễ đến mức ngôn ngữ này trở nên rất hấp dẫn và dễ chịu khi vừa mới ló mặt.

Dù JavaScript có lẽ là ngôn ngữ dễ nhất để bạn cài đặt môi trường và chạy thử 1 chương trình, nhưng chuyện thành thạo ngôn ngữ này lại là điều hiếm gặp hơn nhiều so với các ngôn ngữ khác. Nếu để viết được một chương trình hoàn thiện với C hay C++, bạn sẽ cần phải hiểu cặn kẽ về mọi thứ của ngôn ngữ lập trình đó. Đối với JavaScript, thường là người ta chẳng bao giờ vượt quá lớp bề mặt những điều mà ngôn ngữ này có thể thực hiện được.

Thay vì phô diễn sự phức tạp trong các khái niệm, ngôn ngữ này lại chỉ để lộ ra những thứ *có vẻ* rất đơn giản, ví dụ như chuyện bạn có thể truyền các functions vào làm callbacks một cách dễ dàng, điều này làm cho các lập trình viên có thể sử dụng nó một cách đơn giản, và hoàn toàn không cần phải để ý gì đến sự phức tạp của những thứ đang chạy phía dưới.

Nó vừa là một ngôn ngữ đơn giản, dễ sử dụng, được ưa chuộng một cách rộng rãi, đồng thời cũng có rất nhiều cơ chế của nó mà nếu không nghiên cứu một cách cẩn thận bạn sẽ không thể *thực sự hiểu được* cho dù bạn có là một lập trình viên JavaScript lâu năm đi chăng nữa.

Bởi vì JavaScript *có thể* được sử dụng mà không cần hiểu cặn kẽ, nên việc hiểu cặn kẽ về ngôn ngữ này thường bị bỏ qua.

## Sứ mệnh

Nếu mỗi lần bạn cảm thấy bất ngờ hoặc thất vọng về JavaScript, và bạn phản ứng lại bằng cách cho nó vào danh sách đen, giống như nhiều người vẫn thường làm vậy, thì tất cả những thứ mà bạn biết về JavaScript chỉ là một cái vỏ trống rỗng.

Phần này hay được gọi là "phần hay" của ngôn ngữ, nhưng tôi mong bạn, đọc giả thân mến, hãy coi nó là "phần dễ", "phần đơn giản" hay thậm chí là "phần chưa hoàn thiện".

Bộ sách "Bạn đếch biết JavaScript" sẽ cho bạn một thử thách ngược lại: học và hiểu thật kỹ *toàn bộ* về JavaScript, và đặc biệt là phần "khó nhằn" của ngôn ngữ này.

Ở đây, chúng ta đề cập đến một xu hướng của các lập trình viên JS chỉ học vừa đủ để làm cho xong việc, mà không bao giờ tự bắt mình phải học hiểu chính xác tại sao và làm thế nào mà JS lại chạy như vậy. Xa hơn nữa, chúng ta tránh bỏ những lời khuyên thường gặp để học cách phản ứng lại khi con đường của chúng ta đi trở nên không dễ dàng.

Tôi không hài lòng về việc dừng tay khi thấy thứ gì đó vừa *chạy được* mà không thực sự hiểu *tại sao*. Tôi nghĩ bạn cũng nên thế. Tôi trân trọng được khích lệ bạn đặt chân mình vào "con đường ít người đi" và nắm vào tay mình tất cả kiến thức về những điều mà JavaScript có và có thể làm được. Với những kiến thức đó, sẽ không còn kỹ thuật nào, framework nào hay ám hiệu nào còn nằm ngoài sự hiểu biết của bạn.

Mỗi cuốn sách trong bộ sách này được lấy từ những phần cốt lõi của ngôn ngữ JS nhưng lại thường bị hiểu sai hoặc hiểu thiếu, chúng ta sẽ đào sâu và đầy đủ vào những điều đó. Sau khi học xong, bạn sẽ có được một sự tự tin vững chắc về kiến thức của bản thân, không chỉ là lý thuyết mà còn cả từng bit của "những điều bạn cần biết".

Những thứ về JavaScript mà bạn biết *cho đến lúc này* có lẽ chỉ là những điều do ai đó truyền lại, một người mà chính hiểu biết của họ cũng trong trạng thái không đầy đủ. Thứ JavaScript đó chỉ là cái bóng của cả một ngôn ngữ. Bạn vẫn chưa *thực sự* hiểu được nó, vẫn chưa đâu, nhưng khi bạn đọc vào bộ sách này, bạn sẽ hiểu được. Hãy đọc tiếp đi, bạn của tôi. JavaScript đang chờ đón bạn.

## Lời kết

JavaScript là một thứ tuyệt vời. Dễ dàng để học một phần, nhưng rất khó nhằn nếu muốn học trọn vẹn, hay thậm chí chỉ là học *cho đủ*. Khi gặp vấn đề, lập trình viên thường kêu ca về JS thay vì sự thiếu hiểu biết của chính mình. Những tập sách này mong muốn thay đổi điều đó và lan toả sự trân trọng cho một ngôn ngữ mà từ giờ bạn có thể hiểu và nên hiểu một cách sâu sắc.

Ghi chú: Nhiều ví dụ trong bộ sách này được viết với giả thiết là bạn đang sử dụng các JavaScript engine mới nhất (thậm chí là tương lai), ví dụ như ES6. Những đoạn code đó có thể không chạy được trong các phiên bản môi trường thực thi JS cũ hơn (trước ES6).
