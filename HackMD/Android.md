---
title: Android
---

# Android
## Learn
- Make it visual: học với hình ảnh
- Use a conversational and personalized style: Hãy sử dụng một cách viết thân thiện và cá nhân hóa ( theo ngôn ngữ tự nhiên)
- **Thúc đẩy người học suy nghĩ sâu hơn.** Nói cách khác, trừ khi bạn tự tích cực sử dụng các tế bào thần kinh của mình, thì không có gì xảy ra nhiều trong đầu bạn. Người đọc phải được động viên, tham gia, tò mò và được truyền cảm hứng để giải quyết vấn đề, rút ra kết luận và tạo ra kiến thức mới. Và để làm điều đó, bạn cần các thách thức, bài tập, và câu hỏi kích thích tư duy, cùng với các hoạt động liên quan đến cả hai bên não và nhiều giác quan.
- **Học nhanh hơn khi hứng thú**
- **Chạm đến cảm xúc**
- Nếu bạn thực sự muốn học và muốn học nhanh hơn, sâu hơn, hãy chú ý đến cách bạn chú ý. Hãy suy nghĩ về cách bạn nghĩ. Tìm hiểu cách bạn học.
- Học và ghi nhớ nhiều hơn khi thực hành
---
1. **Slow down. The more you understand, the less you have to memorize.**
- Don’t just read. Stop and think. When the book asks you a question, don’t just skip to the answer. Imagine that someone really is asking the question. The more deeply you force your brain to think, the better chance you have of learning and remembering.
2. **Do the exercises. Write your own notes**
- We put them in, but if we did them for you, that would be like having someone else do your workouts for you. And don’t just look at the exercises. Use a pencil. There’s plenty of evidence that physical activity while learning can increase the learning.
3. **Read “There Are No Dumb Questions.”**
That means all of them. They’re not optional sidebars, they’re part of the core content! Don’t skip them.
4. **Make this the last thing you read before bed. Or at least the last challenging thing**
Part of the learning (especially the transfer to long-term memory) happens after you put the book down. Your brain needs time on its own, to do more processing. If you put in something new during that processing time, some of what you just learned will be lost.
5. **Talk about it. Out loud.**
Speaking activates a different part of the brain. If you’re trying to understand something, or increase your chance of remembering it later, say it out loud. Better still, try to explain it out loud to someone else. You’ll learn more quickly, and you might uncover ideas you hadn’t known were there when you were reading about it.
6. **Drink water. Lots of it.**
Your brain works best in a nice bath of fluid. Dehydration (which
can happen before you ever feel thirsty) decreases cognitive
function.
7. **Listen to your brain.**
Pay attention to whether your brain is getting overloaded. If you find yourself starting to skim the surface or forget what you just read, it’s time for a break. Once you go past a certain point, you won’t learn faster by trying to shove more in, and you might even hurt the process.
8. **Feel something.**
Your brain needs to know that this matters. Get involved with the stories. Make up your own captions for the photos. Groaning over a bad joke is still better than feeling nothing at all.
9. **Write a lot of code!**
There’s only one way to learn to develop Android apps: write a lot of code. And that’s what you’re going to do throughout this book. Coding is a skill, and the only way to get good at it is to practice. We’re going to give you a lot of practice: every chapter has exercises that pose a problem for you to solve. Don’t just skip over them—a lot of the learning happens when you solve the exercises. We included a solution to each exercise—don’t be afraid to peek at the solution if you get stuck! (It’s easy to get snagged on something small.) But try to solve the problem before you look at the solution. And definitely get it working before you move on to the next part of the book.

# Structure
![Screenshot from 2023-11-03 22-41-33.png](https://hackmd.io/_uploads/HyAkH5M7T.png)
![image.png](https://hackmd.io/_uploads/B1ZyKcGXT.png)

---

![image.png](https://hackmd.io/_uploads/r1kXWiMXp.png)

---
# Layout
![image.png](https://hackmd.io/_uploads/BJv0ejXXT.png)

- **`LinearLayout`**: vertical column or a horizontal row
    - ![image.png](https://hackmd.io/_uploads/H19r3r7Qa.png)
- **`Frame layout`**: ui xếp chồng
- **`strings.xml`**: is a String resource file. It’s used to separate out text values from the layouts and activities, and supports localization

![Screenshot from 2023-11-04 11-19-36.png](https://hackmd.io/_uploads/HkGj8BQXa.png)

- **`xmlns:android`**: defined namespace named 'android', Defining this namespace gives your layout access to the elements and attributes that your layout needs, and you need to define it in every layout file you create.

- **`AndroidManifest.xml`**: contain essential infomation about your app
    - name app package
    - detail any of activites
    - permission app requires
    - ![image.png](https://hackmd.io/_uploads/HkzioHQ76.png)

- **`drawable`**: nơi thêm picture vào project
    - ![image.png](https://hackmd.io/_uploads/SkCY-cQma.png)

# Attribute
- `layout_weight`: ~ **flex**
    - đặt layout_height: 0dp -> giúp android không p tính toán lại (layout sẽ quyết định kích thước)
- `android:gravity`:  controls the position of a **view’s contents** (e.g: text in button)
    - ![image.png](https://hackmd.io/_uploads/r1cIuPQm6.png)
    - có thể kết hợp nhiều giá trị = |
- **`android:layout-gravity`**: controls the position of a view within a **layout**
- ![image.png](https://hackmd.io/_uploads/Bye9nwXmp.png)
    - xml -> java
    - setContentView -> convert to Object
![image.png](https://hackmd.io/_uploads/r1yLVtLX6.png)

![image.png](https://hackmd.io/_uploads/BkZyqiQmT.png)
![image.png](https://hackmd.io/_uploads/rkAg9iX7T.png)
![image.png](https://hackmd.io/_uploads/HJhdosmQp.png)

>constraint layouts are part of a suite of libraries known as **Android Jetpack**

# Android Jetpack
![image.png](https://hackmd.io/_uploads/r18cTimm6.png)

- **Gradle**: build tool that’s used to compile code, configure apps, and fetch any extra libraries that your project requires.
    - ![image.png](https://hackmd.io/_uploads/rJUdK0XXp.png)

# Layout
## Constrain
- **Guidle**: cần path tĩnh hoặc cố định 
- **Barrier**: patch động
    -> k được hiển thị khi run app
![image.png](https://hackmd.io/_uploads/BkW8alEQa.png)

![image.png](https://hackmd.io/_uploads/rkGvalV7T.png)
-> ~ flex-wrap

# Active State
![image.png](https://hackmd.io/_uploads/rkXWv_LQa.png)


![Screenshot from 2023-11-05 11-54-36.png](https://hackmd.io/_uploads/ryBXesEQp.png)

- **onCreate()**: được gọi ngay lập tức khi active launched
    - luôn p ghi đè **setContentView()**
- **onDestroy()**: method final call, before activity is destroyed

![image.png](https://hackmd.io/_uploads/BkdxM04Qp.png)

**1. The activity gets launched.**
The activity object is created and its constructor is run.
**2. The onCreate() method runs immediately after the activity is launched.**
The onCreate() method is where any initialization code should go, as this method always gets called after the activity has launched but before it starts running.
**3. An activity is running when it’s visible in the foreground and the user can interact with it.**
This is where an activity spends most of its life
**4. The onDestroy() method runs immediately before the activity is destroyed**
The **onDestroy() method** enables you to perform any final
cleanup such as freeing up resources.
**5. After the onDestroy() method has run, the activity is destroyed.**
The activity ceases to exist.

![image.png](https://hackmd.io/_uploads/Sy7UmCVQ6.png)
## Bundle
- Để khi xoay màn hình MainActivity bị destroyed và recreate -> ta sẽ save state vào Bundle trước khi destroyed
- A **Bundle** is a type of object that’s used to hold key/value pairs. 
![image.png](https://hackmd.io/_uploads/HJ6LBAEQT.png)
    - put+Type (bundle.putInt(key, value))
    - get+Type (bundle.getInt(key))
- **Bundle** are passed from one instance of an activity to another using two methods: **onSaveInstanceState()** and **onCreate()**.
    - **onSaveInstanceState()**: add values to Bundle before the activity is destroyed
    - **onCreate()**: pick Bundle when activity recreated

![Screenshot from 2023-11-05 16-06-34.png](https://hackmd.io/_uploads/HkILsAN7p.png)
![Screenshot from 2023-11-05 16-46-10.png](https://hackmd.io/_uploads/HJT_EyHXT.png)

## Question
- tại sao onCreate lại reCreate khi xoay màn hình
    -  Phương thức onCreate() thường được sử dụng để thiết lập giao diện. Nếu mã của bạn trong onCreate() phụ thuộc vào cấu hình màn hình (ví dụ, nếu bạn có các bố cục khác nhau cho chế độ ngang và chế độ dọc), thì bạn muốn phương thức onCreate() được gọi mỗi khi cấu hình thay đổi. Ngoài ra, nếu người dùng thay đổi ngôn ngữ của họ, bạn có thể muốn tạo lại giao diện người dùng bằng local language.

# OnStart() and onStop()
![image.png](https://hackmd.io/_uploads/rkmAEyHXT.png)

:::info
**When you override any activity lifecycle method in your activity, you need to call the Activity superclass method or the compiler will complain.**
:::

- **onStart()** gets called whenever the activity becomes visible.
- **onRestart()** only gets called when the activity becomes visible again after losing visibility.
- onStop(): được gọi khi activity bị ngừng hiển thị nhưng vẫn hoạt động (ví dụ đang ở màn hình hiển thị đồng hồ -> quay lại home -> onStop được chạy)

## onPause() and onResume()
![image.png](https://hackmd.io/_uploads/rkayUZrmT.png)

![image.png](https://hackmd.io/_uploads/rJgbUZBm6.png)

- when an activity is visible but doesn’t have the focus -> activity **paused** 
    - This can happen if another activity appears on top of your activity that isn’t full-size or that’s transparent. The activity on top has the focus, but the one underneath is still visible and is therefore paused.

:::info
**An activity has a state of paused if it’s lost the focus but is still visible to the user. The activity is still alive and maintains all its state information.**
:::

- **onPause()**:  gets called when your activity is visible but another activity has the focus
- **onResume()**: is called immediately before your activity is about to start interacting with the user.

# Your handy guide to the activity lifecycle methods

![image.png](https://hackmd.io/_uploads/HkKcPO87a.png)

![image.png](https://hackmd.io/_uploads/Hy1AKOU76.png)

:::info
- **User starts the activity and starts using it**
    - A -> G -> D: The activity is created, then made visible, then receives the focus
    - onCreate -> onStart -> onResume
- **User starts the activity, starts using it, then switches to another app.**
    - A -> G -> D -> B -> E: The activity is created, then made visible, then receives the focus. When the user switches to another app, it loses the focus and is no longer visible to the user.
    - onCreate -> onStart -> onResume -> onPause -> onStop
- **User starts the activity, starts using it, rotates the device, switches to another app, then goes back to the activity.**
    - A -> G -> D -> B -> E -> H -> A -> G -> D -> B -> E -> C-> G -> D: First, the activity is created, made visible, and receives the focus. When the device is rotated, the activity loses the focus, stops being visible, and is destroyed. It’s then created again, made visible, and receives the focus. When the user switches to another app and back again, the activity loses the focus, loses visibility, becomes visible again, and regains the focus
    - onCreate -> onStart -> onResume -> onPause -> onStop -> onDestroy -> onCreate -> onStart -> onResume -> onPause -> onStop -> onRestart -> onStart -> onResume
:::

![image.png](https://hackmd.io/_uploads/H1Q_yYLQ6.png)

# Fragment
![image.png](https://hackmd.io/_uploads/HyyMttLmT.png)


1. When the app is launched, MainActivity gets created
2. MainActivity’s onCreate() method runs.
![image.png](https://hackmd.io/_uploads/rygxcKIX6.png)
3. activity_main.xml includes a FragmentContainerView
![image.png](https://hackmd.io/_uploads/SkKzqKIXp.png)
4.  WelcomeFragment’s onCreateView() method is called, which inflates its layout
![image.png](https://hackmd.io/_uploads/H1IN9t8Qa.png)
5. Finally, MainActivity is displayed on the device
![image.png](https://hackmd.io/_uploads/Sy189Y8m6.png)
