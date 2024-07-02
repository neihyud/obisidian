---
title: Kotlin And Android
tags: [Programming]

---

###### tags: `Programming`
# Kotlin And Android

## Null
- Mặc định biến không thể null

## !!, ? & ?: & :: operators

```kotlin=
- ?, ?:

var fishFoodTreats = 6
fishFoodTreats = fishFoodTreats?.dec() ?: 0
<=>
var fishFoodTreats = 6
if (fishFoodTreats != null) {
    fishFoodTreats = fishFoodTreats.dec()
} else {
    fishFoodTreats = 0
}

- !!
    + chuyển đổi bất kỳ giá trị nào thành kiểu not null và ném một ngoại lệ nếu giá trị là null.

- :: -> chỉ định đối số là một hàm thông thường (truyền tham chiếu dưới dạng đối số không phải là gọi hàm)
fun increaseDirty( start: Int ) = start + 1
println(updateDirty(15, ::increaseDirty))
```

## Arrays, Lists, Loop

- arrayOf(...), intArrayOf(...)
- listOf(...)
- `..`: range

## Functions
- Mọi thứ đều có giá trị ngoại trừ loop, nếu một func return về một giá trị thì giá trị mặc định là `kotlin.Unit`

```kotlin=
fun swim(speed: String = "fast") {
   println("swimming $speed")
}

swim()   // uses default speed
swim("slow")   // positional argument
swim(speed="turtle-like")   // named parameter
```

- Lambdas: ->

## Class và Object
- `init block`: gọi khi khởi tạo, có thể có nhiều init trong một class, bất kỳ prop nào được khai báo trong init block p được khai báo trước

```kotlin=
class Aquarium (var length: Int = 100, var width: Int = 20, var height: Int = 40) {
    init {
        println("aquarium initializing")
    }
    init {
        // 1 liter = 1000 cm^3
        println("Volume: ${width * length * height / 1000} liters")
    }
    ...
}
```

- Visable Modifiers:
    - private: chỉ hiển thị trong class đó
    - protected: hiển thị trong bất kỳ subclass
    - internal: visable trong một module

- Nếu bạn muốn property có thể đọc và viết nhưng bên ngoài chỉ đọc thì cho set() là private
```kotlin=
var volume: Int
    get() = width * height * length / 1000
    private set(value) {
        height = (value * 1000) / (width * length)
    }
```

## Subclass và Inheritance
- Kotlin mặc định không thể có subclass, p có keyword `open` cho phép có subclass, cả property ghi đè cũng p có `open` ở parent class
- Subclass phải khai báo tham số constructor một cách rõ ràng
- Muốn ghi đè property sử dụng `override`
- Tạo một data class bằng cách bắt đầu khai báo với `data`. 
- *Destructuring*  là một cách viết tắt để gán các thuộc tính của một data object cho các biến riêng biệt. 
- Tạo một lớp singleton bằng cách sử dụng `object` thay vì `class`

## Pairs/triples, collections, constants, and writing extension functions

- Companion Object: tương tự như static Object

![](https://i.imgur.com/8LvOGKA.png)

- Sự khác nhau giữa `const val` và `val`
```kotlin=
 Giá trị cho const val được xác định tại thời điểm biên dịch, 
trong khi giá trị cho val được xác định trong quá trình thực 
thi chương trình, có nghĩa là, val có thể được gán bởi một hàm 
tại thời điểm chạy.
```




# Android
## ViewGroup

![](https://i.imgur.com/RVcOsyp.png)
- Use a **FrameLayout** if you have only one child View. 
    - 
- Use a **LinearLayout** to display views in a row or column. 
    - Set android:orientation to horizontal or vertical
    ```xml=
    <LinearLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">
    <TextView ... />
    <TextView ... />
    <Button ... />
</LinearLayout>
    ```
- Use a **ConstraintLayout** for more complex layouts. 

## Resource

main
├── java
└── res
   ├── drawable
   ├── layout
   ├── mipmap
   └── values

- The **drawable** directory stores all files related to drawing images and related assets.
- The **layout** directory contains all the layout XML files for your application. There may be more than one if your app needs to handle different orientations or densities.
- The **mipmap** directory contains files for your app icon (known as your launcher icon) at different screen densities.
- The **values** directory contains files related to simple collections of strings, colors, integers, and styles. If you are localizing your app, you might have more than one values directory.

## Resource IDs
- mỗi một resource sẽ có một resource id để truy cập
- tham chiếu đến các mục con
    - **R.<resource_type>.<resource_name>**
    - 
## Activities

- Một Activity: hoàn thành một mục đích chính

![](https://i.imgur.com/Rw0PmGo.png)

- Tham chiếu tói một view
    - e.g: **val resultTextView: TextView = findViewById(R.id.textView)**
- View.OnClickListener:
```kotlin=
- SAM (single abstract method): là một cách khác để định nghĩa OnClickListener
```
![](https://i.imgur.com/Jj6wkS8.png)

## Late initialization

- Khi tạo ứng dụng, bạn sẽ thấy mình cần tạo các thuộc tính hoặc trường có thể phụ thuộc vào các lệnh gọi hàm như onCreate. Một tùy chọn là đặt kiểu là nullable và đặt giá trị ban đầu thành null. Điều đó có nghĩa là mỗi lần sử dụng sẽ cần phải kiểm tra null. Bạn có thể tránh phải làm điều này bằng cách sử dụng lateinit, nó chỉ cho trình biên dịch Kotlin biết rằng bạn chịu trách nhiệm về việc khởi tạo. Nếu bạn cố gắng sử dụng trường hoặc thuộc tính lateinit của mình trước khi khởi tạo, một ngoại lệ sẽ được đưa ra.

## Grandle: Building an Android app

- Xây dựng một hệ thống tự động hóa
- Quản lý phụ thuộc giữa project và thư viện bên thứ ba
- Grandle build file:
    - Declare Plugins
    - Define Android properties
    - Handle dêpndencies
    - Connect to repositories
        - Out on the web, there are collections of libraries organized in repositories. Use the repositories block to specify where to look for them. Gradle will start at the first listed repository and fall through to each subsequent one if it doesn’t find the resource.
### Common Grandle tasks
- Clean
    - The **clean** task deletes all compiled files from the build directory. 
- Tasks
    - **Tasks** outputs a list of tasks for your project and any installed plugins. 
- InstallDebug
    - **InstallDebug** compiles the app if necessary, builds a debug APK, and installs it on a connected physical or emulated device. 

### Accessibility
- Set **contentDescription** attribute → read aloud by screen reader

### Learn more
- Layouts: https://developer.android.com/guide/topics/ui/declaring-layout
- LinearLayout: https://developer.android.com/guide/topics/ui/layout/linear
- Input events overview: https://developer.android.com/guide/topics/ui/ui-events.html
- View: https://developer.android.com/reference/kotlin/android/view/View
- ViewGroup: https://developer.android.com/reference/kotlin/android/view/View

## Screen-density buckets

![](https://i.imgur.com/Ma0T88T.png)

## Android View rendering cycle

![](https://i.imgur.com/7nOa0rS.png)

- **Measure** pass: tính toán các kích thước chính xác của mỗi View, bao gồm các hoạt động như `wrap_contents`
- **Layout** pass: căn chỉnh các chế độ xem dựa vào layout manager
- **Draw**: hiển thị kết quả dựa vào hai bước trước đó

## ConstraintLayout

- Relative positioning constraints:
    - **Format**: layout_constraint<SourceConstraint>_to<TargetConstraint>Of

- Constraint Widget in Layout Editor
![](https://i.imgur.com/Zn0gzj4.png)

- Use **match_constraint**
    - không thể sử dụng **match_parent** trong một child view, thay vào đó sử dụng **match_constraint**
![](https://i.imgur.com/CY8TUaU.png)

- Chain styles
![](https://i.imgur.com/eIum4hx.png)

## Data binding
    
- Tránh p sử dụng findById() => dễ gây lỗi => data binding
- Để sử dụng
    - Modify build.gradle file:
    ```xml=
    android {
       ...
       buildFeatures {
           dataBinding true
       }
    }
    ```
    - Add layout tag:
    ```xml=
    <layout>
        <androidx.constraintlayout.widget.ConstraintLayout>
            <TextView ... android:id="@+id/username" />
            <EditText ... android:id="@+id/password" />
        </androidx.constraintlayout.widget.ConstraintLayout>
    </layout>
    
    - binding class sẽ chứa tham chiếu đên bất kỳ View có một resource ID
    ```

## Data binding layout variables
    
```xml=
<layout> 
   <data>
       <variable name="name" type="String"/>
   </data>
   <androidx.constraintlayout.widget.ConstraintLayout>
       <TextView
           android:id="@+id/textView"
           android:text="@{name}" />
   </androidx.constraintlayout.widget.ConstraintLayout>
</layout>

    
In MainActivity.kt:
binding.name = "John"
    
=> Gọi biến `name` ở TextView
```

## Displaying lists with RecyclerView

- RecyclerView:
    - Widget để hiển thị một list data
    - "Redcycles" (reusess) để làm cho việc scrolling hiệu quả hơn
    - Hỗ trợ animatinos and transtitions

- RecyclerView.Adapter:
    - Cung cấp dữ liệu và layouts mà RecyclerView hiển thị
    - A custom Adapter extends from RecyclerView.Adapter and overrides these three functions:
        - getItemCount: trả về tổng số item trong một list data
        - onCreateViewHolder: được gọi để cập nhập lại một list item layout mới
        - onBindViewHolder: được gọi khi tái sử dụng một list item layout bằng cách update lại data hiển thị trong nó
    - Một **ViewHolder**: đại diện cho một list item layout và có tham chiếu tới tất cả views bên trong list item layout

- Add RecyclerView to your layout
```xml=
<androidx.recyclerview.widget.RecyclerView
    android:id="@+id/rv"
    android:scrollbars="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"/>
```
- Create a list item layout
```xml=
res/layout/item_view.xml
    
<FrameLayout
   android:layout_width="match_parent"
   android:layout_height="wrap_content">
   <TextView
       android:id="@+id/number"
       android:layout_width="match_parent"
       android:layout_height="wrap_content" />
</FrameLayout>

- Create a new layout file that represents a list item layout (in this case a FrameLayout that contains a single TextView). This list item layout is used for each entry in the list.
```

## Create a list adapter
    
```kotlin=
class MyAdapter(val data: List<Int>) : RecyclerView.Adapter<MyAdapter.MyViewHolder>() {

class MyViewHolder(val row: View) : RecyclerView.ViewHolder(row) {
       val textView = row.findViewById<TextView>(R.id.number)
   }

   override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): MyViewHolder {
       val layout = LayoutInflater.from(parent.context).inflate(R.layout.item_view,
                    parent, false)
       return MyViewHolder(layout)
   }
   override fun onBindViewHolder(holder: MyViewHolder, position: Int) {
       holder.textView.text = data.get(position).toString()
   }
   override fun getItemCount(): Int = data.size
    
```
=> Create a list adapter to create list items for the **RecyclerView**
    1. Create a new Adapter class (MyAdapter) that extends from RecyclerView.Adapter. 
    2. Define a custom ViewHolder class that will hold the views in the layout for a single list item.
    3. Override the 3 methods from RecyclerView.Adapter class: 
        - onCreateViewHolder
        - onBindViewHolder
        - getItemCount

## Set the adapter on thẻ RecyclerView
```kotlin=
In MainActivity.kt:
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)

    val rv: RecyclerView = findViewById(R.id.rv)
    rv.layoutManager = LinearLayoutManager(this)

    rv.adapter = MyAdapter(IntRange(0, 100).toList())
}
```
Then make these changes in the MainActivity file.

1. Find a reference to the RecyclerView in the layout. 
2. Set a Layout Manager on it. LinearLayoutManager or GridLayoutManager are standard ones provided for you, but you can define your own. 
3. Initialize an instance of your custom adapter, and set the adapter on the RecyclerView so that the adapter can populate the list items in the RecyclerView.


## App navigation  
## Multiple activities and intents
- **Intent**: yêu cầu một hành động từ một app component khác, vd: another Activity
    - Một **Intent** thường có hai phần thông tin chính
        - Action để thực hiện (vd: ACTION_VIEW, ACTINO_EDIT)
        - Dữ liệu để hoạt động
    - Được sử dụng để chỉ định yêu cầu chuyển đổi sang other Activity
- **Explicit intent**: điều hướng giữa các components
```kotlin=
Navigate between activities in your app:
fun viewNoteDetail() {
   val intent = Intent(this, NoteDetailActivity::class.java)
   intent.putExtra(NOTE_ID, note.id)
   startActivity(intent)
}

    
Navigate to a specific external app:
fun openExternalApp() {
   val intent = Intent("com.example.workapp.FILE_OPEN")
   if (intent.resolveActivity(packageManager) != null) {
       startActivity(intent)
   }
}

```
- resource: https://developer.android.com/guide/components/intents-filters#ExampleExplicit
    
## App bar, navigation drawer, and menus 

- App Bar: top của màn hình
![](https://i.imgur.com/7oAAHbN.png)

- Navgation drawer
![](https://i.imgur.com/2wagkDF.png)
     - resource: 
        - https://developer.android.com/training/appbar/action-views
        - https://material.io/components/navigation-drawer#usage
- Menu
```xml=
Define menu items in XML menu resource (located in res/menu folder) 

<menu xmlns:android="http://schemas.android.com/apk/res/android"
   xmlns:app="http://schemas.android.com/apk/res-auto">

   <item android:id="@+id/action_intent"
       android:title="@string/action_intent" />

   <item
       android:id="@+id/action_settings"
       android:orderInCategory="100"
       android:title="@string/action_settings"
       app:showAsAction="never" />
</menu>
```
![](https://i.imgur.com/BwnKF51.png)

- Inflate options menu
```kotlin=
override fun onCreateOptionsMenu(menu: Menu): Boolean {
    menuInflater.inflate(R.menu.main, menu)
    return true
}
```
    resource Create Options Menu: https://developer.android.com/guide/topics/ui/menus#options-menu
    
- Handle menu options selected
```kotlin=
override fun onOptionsItemSelected(item: MenuItem): Boolean {

    when (item.itemId) {
        R.id.action_intent -> {
            val intent = Intent(Intent.ACTION_WEB_SEARCH)
            intent.putExtra(SearchManager.QUERY, "pizza")
            if (intent.resolveActivity(packageManager) != null) {
                startActivity(intent)
            }
        }
        else -> Toast.makeText(this, item.title, Toast.LENGTH_LONG).show()
    ...
```

## Fragments
- Đại diện một hành vi hoặc một phần của UI trong một activity
- Phải được tổ chức trong một activity
- Vòng đời gắn liền với vòng đời hoạt động của máy chủ
- Có thể thêm hoặc xóa trong thời gian chạy

## Navigation component
- Bao gồm 3 phần chính:
    - Navigation graph: đại diện cho một tập hợp các fragment hoặc các hoạt động mà ứng dụng có thể hiển thị <=> Giống danh sách các chương trình có sẵn để xem
    - Navigation Host là một container chứa list navigation graph <=> Giống TV hoặc màn hình điều khiển
    - Navigation Controller: là phương tiện điều hướng giữa các điểm đến <=> điều khiển TV chuyển sang kênh khác

- Để bắt đầu làm việc với Navigation component
    - add dependencies
    ```kotlin=
    In build.gradle, under dependencies:
    implementation "androidx.navigation:navigation-fragment-ktx:$nav_version"
    implementation "androidx.navigation:navigation-ui-ktx:$nav_version"
    ```

- Navigation Host(NavHost)
```kotlin=
<fragment
    android:id="@+id/nav_host"
    android:name="androidx.navigation.fragment.NavHostFragment"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    **app:defaultNavHost**="true"
    **app:navGraph**="@navigation/nav_graph_name"/>
```

## Create a Fragment
- Extend Fragment class
- Override **onCreateView()**
- Inflate a layout for the Fragment that you have defined in XML
```kotlin=
class DetailFragment : Fragment() {

   override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?,
           savedInstanceState: Bundle?): View? {
       return inflater.inflate(R.layout.detail_fragment, container, false)
   }
}
```
## Chỉ định đích đển cho một Fragment
- Đích đến của một Fragment được biểu thị bằng tag `action` trong navigation graph
- ID hành động được tạo tự động có dạng
**action_<sourceFragment>_to_<destinationFragment>.** 
    
```xml=
- e.g    

<fragment
    android:id="@+id/welcomeFragment"
    android:name="com.example.android.navigation.WelcomeFragment"
    android:label="fragment_welcome"
    tools:layout="@layout/fragment_welcome" >

    <action
        **android:id=**"@+id/action_welcomeFragment_to_detailFragment"
        **app:destination=**"@id/detailFragment" />

</fragment>

```

## Navigation Controller (NavController)
- NavController quản lý UI navigation trong một navigation host
```kotlin=
class MainActivity : AppCompatActivity() {

   override fun onCreate(savedInstanceState: Bundle?) {
       ...
       val navController = findNavController(R.id.myNavHostFragment)
   }
   fun navigateToDetail() {
       navController.navigate(R.id.action_welcomeFragment_to_detailFragment)
   }
}
```

## More custom navigation behavior
![](https://i.imgur.com/UM9MMOa.png)

![](https://i.imgur.com/ZHhN1Wk.png)

## Sending data to a Fragment
1. Tạo một agruments tới một fragment sẽ đợi
2. Tạo action để liên kết từ nguồn đến đích.  
3. Set the arguments trong một method action trên <Source>FragmentDirections. 
4. Navigate theo action sử dụng Navigation Controller
5. Nhận arguments trong một fragments đích
    
```xml=
<fragment
    android:id="@+id/multiplyFragment"
    android:name="com.example.arithmetic.MultiplyFragment"
    android:label="MultiplyFragment" >
    <argument
        android:name="number1"
        app:argType="float"
        android:defaultValue="1.0" />
    <argument
        android:name="number2"
        app:argType="float"
        android:defaultValue="1.0" />
 </fragment>
```
    
## Create action from source to destination
![](https://i.imgur.com/TgqAkpI.png)
## Navigating with actions
![](https://i.imgur.com/L0pLrTs.png)

- Trong onClickListener của source fragment,  gọi hàm hành động trên lớp Directions (được gọi là InputFragmentDirections). Vì đích đích (MultiplyFragment) yêu cầu hai đối số, `action` class cho quá trình chuyển đổi đó cũng có hai đối số. Sau khi tập hợp chúng, chúng ta có thể gọi `navigate()` trên NavController.
## Retrieving Fragment arguments (nhận đối số fragment)
```kotlin=
class MultiplyFragment : Fragment() {
   val args: MultiplyFragmentArgs by navArgs()
   lateinit var binding: FragmentMultiplyBinding
   override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
       super.onViewCreated(view, savedInstanceState)
       val number1 = args.number1
       val number2 = args.number2
       val result = number1 * number2
       binding.output.text = "${number1} * ${number2} = ${result}"
   }
}
```

## Navigation UI
- Menus revisited
```kotlin=
override fun onOptionsItemSelected(item: MenuItem): Boolean {
    val navController = findNavController(R.id.nav_host_fragment)
    return item.onNavDestinationSelected(navController) ||
            super.onOptionsItemSelected(item)
}

```
    
## Logging
![](https://i.imgur.com/q3QEW0M.png)

## Fragment lifecycle
    
![](https://i.imgur.com/0rlSBVF.pn
    
    
![](https://i.imgur.com/TqZtLLR.png)

 - onAttach(): gọi khi một fragment gắn với một context
- onCreateView(): 
    - được gọi để fragment khởi tạo UI view
    - Inflate the fragment layout here and return the root view
- onViewCreated(): 
    - gọi khi view hierachy đã được tạo
    - thực hiện bất kỳ quá trình khởi tạo còn lại tại đây (vd khôi phục trạng thái từ Bundle)
    - kết thúc setting UI trong onViewCreated() => views khả dụng ở thời điểm này
- onDestroyView(): 
    - gọi khi view hierarchy của fragment bị xóa
    - gọi khi view(trước khi được tạo từ onCreatView()) đang được tách khỏi fragment    
- onDetach(): 
    - gọi khi fragment không còn được gắn vào host
## Summary of fragment states
![](https://i.imgur.com/Jsa6nSx.png)

![](https://i.imgur.com/7ge8WDr.png)

## Lifecycle-aware components
- Điều chỉnh hành vi dựa trên activity hoặc fragment lifecycle
    - Sử dụng `androidx.lifecycle` library
    - **Lifecycle** theo dõi lifecycle state của một activity hoặc một fragment
        - Giữ lifecycle state hiện tại
        - Gửi các lifecycle events ( khi có sự thay đổi state)

## ViewModel
- Gradle: lifecycle extensions
```xml=
In app/build.gradle file:

dependencies {
    implementation "androidx.lifecycle:lifecycle-viewmodel-ktx:$lifecycle_version"
    implementation "androidx.activity:activity-ktx:$activity_version"
}
```

![](https://i.imgur.com/FAtoMmT.png)

## Data binding
    
![](https://i.imgur.com/qbM7yGG.png)

- Data binding in XML revisited
```xml=
<layout>
   <data>
       <variable>
           name="viewModel"
           type="com.example.kabaddikounter.ScoreViewModel" />
   </data>
   <ConstraintLayout ../>
</layout>

```
- Attaching a ViewModel to a data binding
```kotlin=
class MainActivity : AppCompatActivity() {

    val viewModel: ScoreViewModel by viewModels()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val binding: ActivityMainBinding = DataBindingUtil.setContentView(this,
            R.layout.activity_main)

      **  binding.viewModel = viewModel**
        ...
```

- Using a ViewModel from a data binding
```kotlin=
In activity_main.xml:

<TextView
    android:id="@+id/scoreViewA"
    android:text="@{viewModel.scoreA.toString()}" />
        ...

```
- ViewModels and data binding
    - khi click button, cần update TextView vì android:text chỉ cài đặt khi views được khởi tạo
```kotlin=
override fun onCreate(savedInstanceState: Bundle?) {
    ...
   val binding: ActivityMainBinding = DataBindingUtil.setContentView(this,
        R.layout.activity_main)

   binding.plusOneButtonA.setOnClickListener {
       viewModel.incrementScore(true)
       binding.scoreViewA.text = viewModel.scoreA.toString()
   }
}
```
    
## LiveData
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
# Attr

- **wrap_content**:  chiều cao của view phụ thuộc vào content của view.
- **match_parent**: chiều cao của view bằng đúng chiều cao của phần tử cha chứa nó
:::info
- **layout_constraintStart_toStartOf: "parent"**: ràng buộc cạnh bắt đầu của phần tử với cạnh bắt đầu của phần tử cha
=> Start - End: chiều ngang, Top - Bottom: dọc
:::
- getRight(), getBottom(), ...: lấy tọa độ bên phải đại diện cho hình chữ nhật
- getMeasuredWidth() và getMeasuredHeight(): kích thước đo được
- `vararg`: số lượng đối số có thể thay đổi

![](https://i.imgur.com/7bMWMmf.png)

![](https://i.imgur.com/jOlpDtJ.png)

![](https://i.imgur.com/sdBIZQn.png)

![](https://i.imgur.com/W4mxG2X.png)

![](https://i.imgur.com/SDCd10T.png)



# **Truy cập file values/string**
- R.string.name_id: lấy text dùng cho android

![](https://i.imgur.com/hWJcJi6.png)
    

# Tự động import

![](https://i.imgur.com/mr8c1Za.png)



**onCreate**: Thay vì gọi hàm main(), hệ thống Android sẽ gọi phương thức onCreate() của MainActivity trong lần đầu mở ứng dụng.

- srcCompat: chỉ hiển thị ở chế độ design
- Sử dụng **setImageResource()** để thay đổi hình ảnh xuất hiện trong một ImageView
- open trước class parent để subclass có thể kế thừa
- "with" là từ để viết tắt đại diện cho một tham chiếu đến một đối tượng

```javascript=
class A {
    val b = B();
    b.compnent1 = something1()
    b.component2 = something2()
    parent.children.add(b)
}

=> Với `with`
class A {
    val b = B();
    
    with(b) {
        component1 = something1()
        component2 = something2()
        parent.children.add(this)
    }
}

```


:::info
- Tạo một lớp abstract mà trong đó một số chức năng còn lại sẽ được các lớp con của lớp đó thực hiện. Do đó, bạn không thể tạo thực thể cho một lớp abstract.
- Tạo lớp con của lớp abstract.
- Sử dụng từ khoá override để ghi đè thuộc tính và hàm trong lớp con.
- Sử dụng từ khoá super để tham chiếu các hàm và thuộc tính trong lớp mẹ.
- Làm cho một lớp trở thành open để có thể tạo lớp con.
- Làm cho một thuộc tính trở thành private để chỉ có thể được sử dụng bên trong lớp đó.
- Sử dụng cấu trúc with để thực hiện nhiều lệnh gọi trên cùng một thực thể đối tượng.
- Nhập chức năng từ thư viện kotlin.math
:::


1. Có thể thấy thẻ của ConstraintLayout là androidx.constraintlayout.widget.ConstraintLayout thay vì ConstraintLayout như TextView. Điều này là do ConstraintLayout là một phần của Android Jetpack, chứa các thư viện mã nhằm cung cấp các chức năng bổ sung cho nền tảng Android cốt lõi. Jetpack cung cấp các chức năng hữu ích, cho phép bạn xây dựng ứng dụng dễ dàng hơn. Bạn sẽ nhận ra thành phần giao diện người dùng này là một phần của Jetpack vì thành phần này bắt đầu bằng "androidx".
2. Bạn có thể đã thấy các dòng lệnh bắt đầu bằng xmlns:, theo sau là android, app và tools.
```javascript=
xmlns:android="http://schemas.android.com/apk/res/android"
xmlns:app="http://schemas.android.com/apk/res-auto"
xmlns:tools="http://schemas.android.com/tools"
```


:::info
Lưu ý: Tên của các điều kiện ràng buộc phải tuân theo mẫu định dạng layout_constraint<Source>_to<Target>Of, trong đó <Source> đề cập đến thành phần hiển thị hiện tại. <Target> đề cập đến cạnh của thành phần hiển thị đích mà thành phần hiển thị hiện tại đang bị ràng buộc vào, hoặc vùng chứa mẹ hoặc một thành phần hiển thị khác.
:::
    
    
![](https://i.imgur.com/w6Qz0m5.png)
![](https://i.imgur.com/SS9aW8U.png)
![](https://i.imgur.com/HgHWR6a.png)
