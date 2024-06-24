---
title: Flutter
tags: [Programming]

---

###### tags: `Programming`
# Flutter
- `pubspec.yaml`: 

f    ![](https://i.imgur.com/KH30MCH.png)


```flutter=
import 'package:flutter/material.dart';


void main() => runApp(MyApp());


class MyApp extends StatelessWidget {
   // This widget is the root of your application.
   @override
   Widget build(BuildContext context) {
      return MaterialApp(
         title: 'Hello World Demo Application',
         theme: ThemeData(
            primarySwatch: Colors.blue,
         ),
         home: MyHomePage(title: 'Home page'),
      );
   }
}
class MyHomePage extends StatelessWidget {
   MyHomePage({Key key, this.title}) : super(key: key);
   final String title;


   @override
   Widget build(BuildContext context) {
      return Scaffold(
         appBar: AppBar(
            title: Text(this.title),
         ),
         body: Center(
            child:
            Text(
               'Hello World',
            )
         ),
      );
   }
}

```
![](https://i.imgur.com/ih6Gxep.png)

## Các thành phần của giao diện mobile

![](https://i.imgur.com/mbiOfry.png)

![](https://i.imgur.com/lVxo9kq.png)



## Widgets
- Trong Flutter, mọi giao diện đều được xây dựng bằng các widget. Widget là một đối tượng có thể được vẽ lên màn hình và có thể có một hoặc nhiều widget con bên trong nó. Widget cung cấp các tính năng để hiển thị văn bản, hình ảnh, tạo các nút bấm, các trường nhập liệu, v.v...
- Tạo widget mới thường kế thừa `
    - `StatelessWidget`:  cho phép bạn tạo ra các widget không thể thay đổi trạng thái (stateless widget). Điều này có nghĩa là khi bạn sử dụng StatelessWidget, widget sẽ hiển thị một giao diện tĩnh mà không thể thay đổi trạng thái và bạn không thể cập nhật giao diện của widget.
    StatelessWidget được sử dụng khi bạn muốn hiển thị các widget không thay đổi trạng thái, ví dụ như các button đơn giản, static text, hình ảnh tĩnh, hoặc các widget tĩnh khác.
    - `StateFullWidget`: một widget có thể thay đổi trạng thái. Khi một widget được xây dựng bằng StatefulWidget, widget sẽ hiển thị một giao diện có thể thay đổi trạng thái và bạn có thể cập nhật giao diện của widget. Ví dụ, bạn có thể sử dụng StatefulWidget để tạo ra một nút bấm và đếm số lần người dùng nhấn vào nút bấm đó.
### Tổng quan về các loại Widget trong Flutter
- Trong Flutter tất các widget được phân loại dựa trên chức năng thành 4 nhóm sau:
	- Các widget giao diện đặc thù theo từng nền tảng - Platform widgets
	- Các widget hỗ trợ bố trí giao diện - Layout widgets
	- Các widget quản lý trạng thái - State maintenance widgets
	- Các widget cơ bản độc lập với nền tảng - Platform independent / basic widgets
### Platform specific widgets
- Đây là các widget dành riêng cho từng nền tảng Android hay IOS
- Các widget dành riêng cho Android được thiết kế theo Material design guideline cho Android OS nên được gọi là  Material widgets.
- Các widget dành riêng cho iOS được thiết kế theo Human Interface Guidelines bởi Apple và được gọi là Cupertino widgets

### Layout widgets
- Layout phổ biến
    - `Container` − Một hình chữ nhật được thiết kế sử dụng BoxDecoration widgets với background (nền), border (đường viền) và shadow (bóng đổ).
    - `Center` − Căn giữa các widget con.
    - `Row` − Sắp xếp các widget con theo hàng ngang (horizontal direction).
    - `Column` − Sắp xếp các widget con theo hàng dọc (vertical direction).
    - `Stack` − Sắp xếp các widget con lên trên cùng (xếp chồng lên nhau), sử dụng Positioned widget để định vị chúng
    - `GridView` - bố trí các widget dưới dạng lưới có thể cuộn
    - `ListView` - đặt các widget dưới dạng list có thể cuộn 
    - 
### Aligning widgets
![](https://i.imgur.com/S1shCg9.png)

![](https://i.imgur.com/I4FAagE.png)

![](https://i.imgur.com/lqVpAbY.png)


### Sizing Sizing widgets
- khi layout quá rộng để vừa với thiết bị thì sử dụng widget `Expanded` (có flex)

### Packing widgets
- mặc định thì row hoặc column chiếm càng nhiều trục main axist càng tốt nhưng nếu bạn muốn dồn các children lại với nhau thì set `mainAxisSize: MainAxisSize.min`

### GridView

- GridView.count - chỉ định số cột
- GridView.extent - chỉ chiều rộng pixel tối đa của tile







### State maintenance widgets
- Widget kế thừa từ `StatelessWidget` sẽ không có bất kì trạng thái nào nhưng nó có thể bao gồm widget được kế thừa từ StatefulWidget

### Platform independent / basic widgets
- **Text**: có thể định dạng văn bản thông qua `TextStyle`
```flutter=
/* Text widget có một hàm constructor riêng, Text.rich, 
sử dụng một TextSpan để mô tả các định dạng. 
TextSpan widget có tính chất đệ quy và nó có thể bao gồm các 
TextSpan khác. Ví dụ: */
Text.rich( 
   TextSpan( 
      children: [ 
         TextSpan(text: "Hello ", style:  
         TextStyle(fontStyle: FontStyle.italic)),  
         TextSpan(text: "World", style: 
         TextStyle(fontWeight: FontWeight.bold)),  
      ], 
   ), 
```
![](https://i.imgur.com/bsZy7ai.png)
![](https://i.imgur.com/G6l0Y3v.png)

---
- **Image**
![](https://i.imgur.com/7bw9XBI.png)

- **Icon**: Icons.name_icon

## Understanding constraints

https://docs.flutter.dev/development/ui/layout/constraints

- **UnconstrainedBox**:  cho phép bạn hiển thị widget con của mình mà không bị ràng buộc bởi các giới hạn kích thước của widget cha
```flutter=
UnconstrainedBox(
  alignment: Alignment.center, // vị trí widget con bên trong
  child: // widget con của bạn
)

```
- **OverflowBox**: Nó cho phép bạn hiển thị nội dung vượt ra ngoài khu vực hiển thị của widget cha, mà không bị cắt hoặc ảnh hưởng đến bố cục của các phần khác của widget.
```flutter=
OverflowBox(
  minWidth: 0.0, // kích thước tối thiểu theo chiều ngang của widget
  maxWidth: double.infinity, // kích thước tối đa theo chiều ngang của widget
  minHeight: 0.0, // kích thước tối thiểu theo chiều dọc của widget
  maxHeight: double.infinity, // kích thước tối đa theo chiều dọc của widget
  alignment: Alignment.center, // vị trí widget con bên trong
  child: // widget con của bạn
)

```
- **ConstrainedBox**: giới hạn kích thước của một widget con bên trong nó. Nó cho phép bạn xác định các ràng buộc kích thước tối thiểu và tối đa cho widget con và nó sẽ tự động thay đổi kích thước của widget con để phù hợp với các ràng buộc đó.
```flutter=
ConstrainedBox(
    constraints: BoxConstraints(
        minWidth: 50.0,
        minHeight: 50.0,
        maxWidth: 100.0,
        maxHeight: 100.0,
    ),
    child: // widget con của bạn
)
```
- **LimitedBox**: giới hạn kích thước của một widget con bên trong nó. Nó được sử dụng để giới hạn kích thước của một widget con dựa trên kích thước của nó bằng cách sử dụng chiều rộng hoặc chiều cao của widget cha.
```flutter=
LimitedBox(
  maxHeight: 100.0,
  maxWidth: 150.0,
  child: // widget con của bạn
)
```
- **FittedBox**: co giãn và điều chỉnh kích thước của một widget con để phù hợp với kích thước của widget cha. Nó có thể được sử dụng để hiển thị hình ảnh hoặc văn bản một cách chính xác và đẹp mắt, bất kể kích thước của widget cha.
```css=
FittedBox(
  fit: BoxFit.contain, // cách hiển thị của widget con bên trong
  alignment: Alignment.center, // vị trí widget con bên trong
  child: // widget con của bạn
)

// Trong đó, fit là một thuộc tính kiểu BoxFit, cho phép bạn điều chỉnh cách
hiển thị của widget con bên trong. Ví dụ, BoxFit.contain sẽ co giãn widget
con bên trong để nó vừa với widget cha, nhưng vẫn giữ tỷ lệ khung hình ban
đầu. alignment là một thuộc tính kiểu Alignment, cho phép bạn điều chỉnh vị
trí của widget con bên trong trong widget cha.

// Khi bạn sử dụng FittedBox, widget con bên trong sẽ được co giãn và thay
đổi kích thước sao cho nó vừa với kích thước của widget cha mà không làm 
biến dạng hoặc làm mất tỷ lệ khung hình ban đầu. Ví dụ, bạn có thể sử dụng
FittedBox để hiển thị một hình ảnh trong một khu vực có kích thước cố định
và giữ nguyên tỷ lệ khung hình ban đầu của hình ảnh.

```
- **Expanded**: cho phép bạn mở rộng một hoặc nhiều widget con để điền vào không gian còn trống trong widget cha.
```flutter=
Expanded(
  flex: 1, // tỉ lệ phần trăm của không gian cha sẽ được sử dụng bởi widget con
  child: // widget con của bạn
)

```
- **Flexible**: cho phép child có kích thước nhỏ hơn hoặc bằng chiều rộng (dài) của flexible
- **Scaffold**: Scaffold là một widget trong Flutter được sử dụng để tạo giao diện cơ bản cho ứng dụng di động. Nó là một thành phần chính của một màn hình trong ứng dụng Flutter và bao gồm nhiều thành phần như AppBar, BottomNavigationBar, FloatingActionButton và Drawer. Scaffold cung cấp một khung cho ứng dụng của bạn, trong đó bạn có thể thêm các thành phần khác nhau để tạo ra giao diện cho ứng dụng của bạn.
- **SizedBox** là một widget trong Flutter cho phép bạn tạo ra một hộp có kích thước được xác định trước

## Managing state



## Layout trong Flutter
- Có hai loại widget layout chính trong Flutter
	- Single Child Widgets - Chỉ có một widget con
	- Multiple Child Widgets - Có nhiều widget con
## Common layout widgets
**Standard widgets**
- Container: Adds padding, margins, borders, background color, or other decorations to a widget.
- GridView: Lays widgets out as a scrollable grid.
- ListView: Lays widgets out as a scrollable list.
- Stack: Overlaps a widget on top of another.

**Material widgets**
- Card: Organizes related info into a box with rounded corners and a drop shadow.
- ListTile: Organizes up to 3 lines of text, and optional leading and trailing icons, into a row.



### Single Child Widgets
- có duy nhất một widget con, cố định, có chức năng duy nhất, vd: button, label
-  Một số single child layout widgets quan trọng trong Flutter 
    - Padding − Được sử dụng để padding child widget. Ở đây, padding có thể sử dụng EdgeInsets class.
    -Align - có thể căn chỉnh bằng `FractionalOffset`, ex: FractionalOffset(1.0, 0.0) biểu thị phía trên bên phải
- Một số single child layout khác:
	- FittedBox
	- Expanded
	- AspectRatio
	- ConstrainedBox
	- Baseline
	- FractinallySizedBox
	- IntrinsicHeight
	- IntrinsicWidth
	- LimitedBox
	- OffStage
	- OverflowBox
	- SizedBox
	- SizedOverflowBox
	- Transform
	- CustomSingleChildLayout

### Multiple Child Widgets
- Một số widget layout dạng này được sử dụng phổ biến
	- Row 
	- Column 
	- ListView 
	- GridView 
	- Table 
	- Flow 
	- Stack 

--- 

**Code Template**
![](https://i.imgur.com/a06YEaL.png)

https://gist.github.com/neihyud/f88899b216a5e4b44457a833316c186e

![](https://i.imgur.com/2oMCPCv.png)



## Gestures
-  `GestureDetector` là một tiện ích không hiển thị trên giao diện nhưng có khả năng nắm bắt các thao tác của người dùng như nhấp, kéo, vuốt, chạm....

- Một số cử chỉ được sử dụng rộng rãi:
	- **Tap** − Chạm vào bề mặt thiết bị bằng đầu ngón tay trong thời gian ngắn sau đỏ thả ngón tay ra ngay
	- **Double Tap** − Tap 2 lần trong thời gian ngắn
	- **Drag** − Chạm vào bề mặt của thiết bị bằng đầu ngón tay và sau đó di chuyển đầu ngón tay một cách ổn định và cuối cùng thả ngón tay ra.
	- **Flick** − Tương tự như drag nhưng thực hiện nhanh hơn.
	- **Pinch** − Chụm bề mặt của thiết bị bằng hai ngón tay
	- **Spread/Zoom** − Ngược lại với Pinch.
	- **Panning** − Chạm vào bề mặt của thiết bị bằng đầu ngón tay và di chuyển nó theo bất kỳ hướng nào mà không nhả đầu ngón tay.

![](https://i.imgur.com/qOuKNL1.png)




## Khái niệm State
- Flutter widgets quản lý các State (trạng thái) thông qua một widget đặc biệt `StatefulWidget`, thay đổi giao diện khi change state
- Quản lý trạng thái trong Widget được chia thành hai loại: 
    - Ngắn hạn: animation (hiệu ứng), thông qua `StatefulWidget`
    - Dài hạn: kéo dài toàn bộ ứng dụng, thông qua `coped_model
`
### StatefulWidget ???
### Coped_model

Cung cấp 3 class chính quản lý trạng thái ứng dụng
- **Model**: có một phương thức duy nhất là notifyListeners, nó được gọi bất cứ khi nào trạng thái của Model thay đổi. notifyListeners sẽ thực hiện các công việc cần thiết để cập nhật giao diện.

```java=

class Product extends Model { 
   final String name; 
   final String description; 
   final int price;
   final String image; 
   int rating; 
   
   Product(this.name, this.description, this.price, this.image, this.rating); 
   factory Product.fromMap(Map<String, dynamic> json) { 
      return Product( 
         json['name'], 
         json['description'], 
         json['price'], 
         json['image'], 
         json['rating'], 
      ); 
   } 
   void updateRating(int myRating) { 
      rating = myRating; notifyListeners(); 
   }
}


```
- **ScopedModel**
    - dễ dàng chuyển Data Model từ widget cha xuống các widget con, cháu của nó
    - rebuild lại các widget con giữ các model mà trong trường hợp model này được cập nhật
    - `ScopeModel.of` là một phương thức dùng để lấy Data Model dưới ScopeModel.
```java=
// Single model
ScopedModel<Product>(
   model: item, child: AnyWidget() 
)
    
// Multiple model
ScopedModel<Product>( 
   model: item1, 
   child: ScopedModel<Product>( 
      model: item2, child: AnyWidget(),
   ),
)
```
- **ScopedModelDescendant**: Data Model từ lớp cha và build lại UI bất kí khi nào Data Model thay đổi.
    - có 2 thuộc tính là builder và child. 
        - `Child` là phần UI không bị thay đổi và sẽ được chuyển cho hàm builder. 
        - Hàm `buider` sẽ  nhận 3 đối số:
            - **Content** :ScopedModelDescendant chuyển sang context của ứng dụng
            - **Child** : Một phần của UI và không thay đổi dựa trên Data Model
            - **Model** : Dưới đây là ví dụ tường minh 



## Layers
- Một `layer` (tầng,lớp) được tạo thành bằng việc sử dụng các `class` tiếp theo ngay canh nó. Top của tất cả các layer là các widget đặc biệt cho Android và iOS. Layer tiếp theo là widget gốc của flutter. Tiếp nữa là `Rendering layer`, đây là level thấp nhất trong việc sinh các thành phần của flutter app. Layer tiếp theo là nền tảng gốc hệ điều hành.

![](https://i.imgur.com/a2wo5fD.png)


## Navigator và Routing
- MaterialPageRoute: render giao diện với một anmation chuyển tiếp nào đó, `MaterialPageRoute(builder: (context) => Widget())` nhận các chức năng build để thay thế context hiện tại  ,với hai phương thức:
    - **Navigator.push()**: `Navigator.push( context, MaterialPageRoute(builder: (context) => Widget()), );` => chuyển tiếp
    - **Navigator.pop()**: `Navigator.pop(context);` => quay lại trang trước
```javascript=
// example
Navigator.push( 
 context, MaterialPageRoute( 
    builder: (context) => ProductPage(item: items[index]), 
 ), 
); 

```
## Animation ??

## Giới thiệu về package
Cấu trúc chung của Package ( ví dụ về package) dưới đây :
 - **lib/src/*** : tệp Dart ở dạng priavte 
 - **lib/my_demo_package.dart** : phần code chính của Dart, có thể thêm một vài ứng dụng 
import 'package:my_demo_package/my_demo_package.dart'
 - Một vài tệp ở dạng private có thể được xuất sang tệp chính (my_demo_package.dart) :
export src/my_private_code.dart
 - **lib/*** : Ta có thể truy cập vào bất kì tệp nào bên trong thư mục  :
import 'package:my_demo_package/custom_folder/custom_file.dart'
 - **pubspec.yaml** : Được hiểu là trình quản lý thư mục của  Package 

Để tích hợp được các gói vào dự án thì ta cần phải  có file **pubspec.yaml**
### Dart Package
 - được lưu trữ và publish trên các máy chủ, https://pub.dev
 - các bước để sử dụng package
```javascript=
-Nhập tên package và phiên bản phù hợp trong file pubspec.yaml như dưới đây :
dependencies: english_words: ^3.1.5
-Bản mới nhất sẽ được cập nhật trên server
- Cài đặt package bằng lệnh :
flutter packages get
- Khi chúng ta đang dùng Android studio, thì Android studio sẽ phát hiện bất kỳ thay đổi trong file pubspec.yaml và hiện thông báo để lập trình viên có thể biết
- Dart package có thể được cài đặt hoặc nâng cấp trong Android studio thông qua menu optionsoptions.
- Thêm các file cần thiết sử dụng lệnh dưới đây và bắt đầu làm việc :
import 'package:english_words/english_words.dart';
- Sử dụng bất kì phương thức có sẵn
nouns.take(50).forEach(print);
- Ở trên ta đã dùng hàm nouns để lấy ra 50 từ đầu tiên

```

### Rest API

Một vài phương thức chính :
- **read** : gửi yêu cầu lên sever thông qua phương thức GET và trả về  Future{String}
- **get** : gửi yêu cầu lên sever thông qua phương thức GET và trả về Future {Response} . Response là lớp giữ lại các thông tin phản hồi 
- **post** : gửi yêu cầu lên sever thông qua phương thức POST  bằng việc đưa giá trị lên sever và phản hồi Future {Response}
- **put** : gửi yêu cầu lên sever thông qua phương thức PUT và trả về phản hồi như Future{Response}
- **head** : gửi yêu cầu lên sever thông qua phương thức HEAD và trả về phản hồi như Future{Response}
- **delete** : gửi yêu cầu lên sever thông qua phương thức DELETE và trả về phản hồi như Future{Response}

## First Project
- SafeArea: đảm bảo child không bị che khuất bỏi phần cứng hoặc thanh trạng thái
- extended in NavigationRail: show labels or not
- selectedIndex: chỉ mục được chọn hiện tại
- onDestinationSelected: sự kiện xảy ra khi người dùng chuyển hướng
- Expanded: thể hiện layout trong đó phần tử con chiếm bố cục nhiều nhất có thể
- LayoutBuider: callback của builder mỗi khi constraints thay đổi
    - người dùng resize app window
    - Người dùng xoay điện thoại của họ từ chế độ dọc sang chế độ ngang hoặc quay lại
    - Một số tiện ích bên cạnh MyHomePagetăng kích thước, làm cho MyHomePagecác giới hạn của ' nhỏ hơn

- ListView: column cuộn
- ListTile: Mỗi ListTile có thể chứa một ảnh (icon), một tiêu đề và một phụ đề.
- leading: dành cho biểu tượng và hình đại diện
- onTap: dành cho tương tác

## Other
- Adaptive: để chạy trên các thiết bị khác nhau
- Responsive: chạy trên các màn hình khác nhau

# Code Sample

## TextButton
```javascript=
TextButton(
  onPressed: () {
    // Do something
  },
  child: Text('Click me'),
  style: ButtonStyle(
    foregroundColor: MaterialStateProperty.all<Color>(Colors.blue),
    backgroundColor: MaterialStateProperty.all<Color>(Colors.white),
    textStyle: MaterialStateProperty.all<TextStyle>(TextStyle(fontSize: 16)),
    padding: MaterialStateProperty.all<EdgeInsets>(EdgeInsets.all(12)),
    shape: MaterialStateProperty.all<RoundedRectangleBorder>(
        RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(10),
            side: BorderSide(color: Colors.blue))),
  ),
)

```
```javascript=
DateTime datetime = DateTime.parse(dateTime!);
// DateTime datetime = DateTime.parse("2023-04-04");
String dayString = "${datetime.day}".padLeft(2, '0');
${datetime.day}".padLeft(2, '0') => 
```

![](https://i.imgur.com/BfsffAM.png)

# Layout
![](https://i.imgur.com/wCgoSlm.png)

![](https://i.imgur.com/c1PaMB2.png)

![](https://i.imgur.com/yf4ol7q.png)

![](https://i.imgur.com/wnqJHVt.png)

![](https://i.imgur.com/c5tdj1N.png)

![](https://i.imgur.com/xj99CAO.png)

![](https://i.imgur.com/HcxqHXn.png)

![](https://i.imgur.com/jxBlhiS.png)

![](https://i.imgur.com/BDcEBlo.png)

![](https://i.imgur.com/SyUEp6R.png)
