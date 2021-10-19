# clean-code-swift

## Mục lục 
  1. [Giới thiệu](#giới-thiệu)
  2. [Biến và Hằng số](#biến-và-hằng-số)
  3. [Hàm](#hàm)
  4. [Closure](#closure)

## Giới thiệu

Hướng dẫn này dựa trên Apple’s excellent Swift standard library style và những phản hồi từ việc sử dụng trên nhiều dự án Swift trong Google. 

## Biến và Hằng số

**Quy ước chung cho các biến, hằng số.**

Có thể viết chung một dòng nếu chúng có liên quan.

```swift
let row: Int, column: Int
```

Trong trường hợp khởi tạo bằng cách ghi giá trị ở phía bên phải, không ghi kiểu dữ liệu ở phía bên trái.

```swift
// ◯
let text = "Hello World!"

// ☓
let text: String = "Hello World!"

// ◯
let flag = false

// ☓
let flag: Bool = false

// ◯
let frame = CGRectZero

// ☓
let frame: CGRect = CGRectZero
```
Tuy nhiên, nếu kiểu dữ liệu không tưởng minh, ta cần thêm kiểu dữ liệu.

```swift
// ◯
let image = UIImage(named: "Hoge")

// ☓
let image: UIImage = UIImage(named: "Hoge")

// ◯
let size = CGSize(10.0, 10.0)

// ☓
let size: CGSize = CGRectZero
```
Trong trường hợp khởi tạo giá trị trả về của phương thức lớp bằng cách sử dụng các kiểu dữ liệu lớp của nó, không ghi kiểu dữ liệu ở phía bên trái vì nó có thể hiểu tương tự.

```swift
let device = UIDevice.currentDevice()

let app = UIApplication.sharedApplication()
```
Trong trường hợp gán ép kiểu, kiểu dữ liệu của định nghĩa biến bị bỏ qua.

```swift
// ◯
let text = data["text"] as String

// ☓
let text: String = data["text"] as String
```

**Định nghĩa biến**

Có nhiều cách để viết định nghĩa mảng trống, mảng kết hợp cùng với cách khởi tạo của chúng, vì vậy chúng ta chỉ nên tham khảo tiêu chuẩn của Apple.

```swift
// Array
// ◯
var elements = [Int]()

// ☓
var elements: [Int] = []

// ☓
var elements: [Int] = [Int]()

// ☓
var elements = Array<Int>()


// Associative array
// ◯
var namesOfIntegers = [Int: String]()

// ☓
var namesOfIntegers: [Int: String] = [Int: String]()

// ☓
var namesOfIntegers = Dictionary<Int, String>()
```

Không khai báo biến toàn cục.
```swift
// ☓
var globalValue: String = "init value"

class Hoge {

}
```

**Định nghĩa hằng số**

Trong trường hợp định nghĩa biến, hãy xem xét liệu nó có thể được định nghĩa như một hằng số hay không. Trừ khi nó phải được định nghĩa là một biến, nếu không hãy định nghĩa nó như một hằng số.

```swift
let notChangeString = "Hoge"
let notChangeArray = ["Hoge", "Fuga"]
let notChangeDictinary = ["Hoge": "Fuga"]
```
Giống như các biến khác, hằng số toàn cục là lowerCamelCase. Ký hiệu Hungary, chẳng hạn như g hoặc k đứng đầu, không được sử dụng.

```swift
// ◯
let secondsPerMinute = 60

// X
let SecondsPerMinute = 60
let kSecondsPerMinute = 60
let gSecondsPerMinute = 60
let SECONDS_PER_MINUTE = 60
```

**[⬆ Về đầu trang](#mục-lục)**

## Hàm

Trong trường hợp kiểu dữ liệu của giá trị trả về là vô hiệu, kiểu dữ liệu sẽ bị bỏ qua.

```swift
// ◯
func chnageWorld(text: String) {  }

// ☓
func chnageWorld(text: String) -> () { }

// ☓
func chnageWorld(text: String) -> Void { }
```

Xem xét khả năng đọc của đối số đầu tiên để sử dụng tên đối số bên ngoài nếu cần.

```swift
// ◯
func dateFromString(_ dateString: String) -> NSDate
dateFromString("2014-03-14") // Có thể suy đoán rằng đối số có thể là chuỗi

// ☓
func dateFromString(dateString: String) -> NSDate
dateFromString(dateString: "2014-03-14") // Dư thừa

// ◯
func convertPointAt(column: Int, row: Int) -> CGPoint
convertPointAt(column: 42, row: 13) 

// ☓
func convertPointAt(_ column: Int, row: Int) -> CGPoint
convertPointAt(42, row: 13) // Đối số đầu tiên ngầm định
```

Có thể bỏ qua tên của đối số đầu tiên và đặt giá trị mặc định, ghi _.
```swift
func log(_ message: String = "default message") 

log("this is message") // -> this is message
log()                  // -> default message
```

**[⬆ Về đầu trang](#mục-lục)**

## Closure

Không viết tắt đối số đóng.

```swift
// ◯
let funcClosure = { (text: String) in
    println(text)
}

// ☓
let funcClosure = { text in
    println(text)
}
```

Trong trường hợp hàng đầu tiên quá dài, hãy ngắt dòng trước đối số.

```swift
let funcClosure = { 
    (text: String) in
    println(text)
}
```

Trong trường hợp xác định kiểu closure khi khởi tạo nó.

```swift
let funcClosure = { (text: String) in
    println(text)
}
funcClosure("Hoge")
```

Trong trường hợp xác định kiểu closure nhưng gán giá trị cho nó sau.

```swift
var funcClosure: ((text: String) -> ())?
funcClosure = { (text: String) in
    println(text)
}
funcClosure!(text: "Hoge")
```

**Trailing Closures**

Các hàm không được overload sao cho hai hàm overload chỉ khác nhau bởi tên đối số closure. Làm như vậy ngăn cản việc sử dụng cú pháp closure - khi nhãn không xuất hiện, lệnh gọi hàm có trailing closure là không rõ ràng.

Ví dụ này cấm sử dụng cú pháp trailing closure để gọi lời greet:

**Không tốt**
```swift
// X
func greet(enthusiastically nameProvider: () -> String) {
  print("Hello, \(nameProvider())! It's a pleasure to see you!")
}

func greet(apathetically nameProvider: () -> String) {
  print("Oh, look. It's \(nameProvider()).")
}

greet { "John" }  // error: ambiguous use of 'greet'
```

**Tốt**
```swift
// ◯
func greetEnthusiastically(_ nameProvider: () -> String) {
  print("Hello, \(nameProvider())! It's a pleasure to see you!")
}

func greetApathetically(_ nameProvider: () -> String) {
  print("Oh, look. It's \(nameProvider()).")
}

greetEnthusiastically { "John" }
greetApathetically { "not John" }
```

Nếu một lệnh gọi hàm có nhiều đối số là closure, thì không có đối số nào được gọi bằng cách sử dụng cú pháp trailing closure; tất cả đều được gắn nhãn và lồng vào bên trong dấu ngoặc đơn của danh sách đối số.

```swift
// ◯
UIView.animate(
  withDuration: 0.5,
  animations: {
    // ...
  },
  completion: { finished in
    // ...
  })
  
 // X
 UIView.animate(
  withDuration: 0.5,
  animations: {
    // ...
  }) { finished in
    // ...
  }
```

Nếu một hàm có một đối số đóng duy nhất và nó là đối số cuối cùng, thì nó luôn được gọi bằng cách sử dụng cú pháp trailing closure, ngoại trừ các trường hợp sau để giải quyết lỗi phân tích cú pháp hoặc không rõ ràng:
1. Như được mô tả ở trên, các đối số closure được gắn nhãn phải được sử dụng để phân biệt giữa hai overload với các danh sách đối số giống hệt nhau.
2. Các đối số closure được gắn nhãn phải được sử dụng trong các câu lệnh luồng điều khiển trong đó phần thân của phần đóng theo sau sẽ được phân tích cú pháp như phần thân của câu lệnh luồng điều khiển.

```swift
// ◯
Timer.scheduledTimer(timeInterval: 30, repeats: false) { timer in
  print("Timer done!")
}

// ◯
if let firstActive = list.first(where: { $0.isActive }) {
  process(firstActive)
}
  
// X
Timer.scheduledTimer(timeInterval: 30, repeats: false, block: { timer in
  print("Timer done!")
})

// X : Ví dụ này không biên dịch được.
if let firstActive = list.first { $0.isActive } {
  process(firstActive)
}
```
