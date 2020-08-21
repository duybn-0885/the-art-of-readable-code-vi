# CHAPTER NINE: Variables and Readability

![](https://i.imgur.com/RD34tXo.png)

Mã nguồn sẽ trở nên vô cùng khó hiểu khi mà việc sử dụng biến bừa bãi, cẩu thả - bạn sẽ nhận thấy điều đó khi mà đọc hết chương này

Đặc biệt, một vài vấn đề mà cần phải quan tâm đó là:

    1. Việc kiểm soát số lượng biến trở nên khó khăn khi mà số lượng quá nhiều
    2. Phạm vi của biến càng lớn, ba
    3. Với các biến thường xuyên thay đổi thì việc kiểm soát giá trị của nó càng khó

3 phần dưới đây sẽ giúp bạn giải quyết các vấn đề nêu trên.
<!-- In this chapter, you’ll see how sloppy use of variables makes a program harder to understand.

Specifically, there are three problems to contend with:
    1. The more variables there are, the harder it is to keep track of them all.
    2. The bigger a variable’s scope, the longer you have to keep track of it.
    3. The more often a variable changes, the harder it is to keep track of its current value. -->

<!-- The next three sections discuss how to deal with these issues. -->

## Loại bỏ bớt các biến
Ở chương 8, Đơn giản hóa các công thức khổng lồ, bạn đã được giới thiệu về việc dùng các biến với vai trò "giải thích" hoặc "tổng kết" công thức, làm mã nguồn dễ đọc hơn. Các biến có tác dụng rất lớn trong việc tách nhỏ các công thức và có thể hiểu đây như là một loại tài liệu.

Trong phần này, bạn sẽ nhận biết được các biến như thế nào sẽ không cải thiện việc đọc hiểu mã nguồn. Khi mà xóa bỏ các biến này sẽ giúp mã nguồn trở nên ngắn gọn, đơn giản hơn.

Trong các phần tiếp theo, sẽ có một vài ví dụ về các biến không cần thiết.

### Sử dụng ít biến tạm thời
Xét ví dụ sau với ngôn ngữ Python, chú ý biến now nhé:
```
now = datetime.datetime.now()
root_message.last_view_time = now
```
Biến now ở đây thật sự có giá trị gì hay không? Chắc là không vì:

- Nó không chia nhỏ biểu thức phức tạp
- Nó không mang ý nghĩa giải thích, datetime.datetime.now() đủ rõ ràng rồi
- Nó được sử dụng một lần duy nhất
Không có biến now, mã nguồn đơn giản chỉ là:
```
root_message.last_view_time = datetime.datetime.now()
```
Các biến như now thường bị sót lại sau khi mã nguồn được chỉnh sửa, có thể  ban đầu chúng được sử dụng ở rất nhiều nơi. Hoặc lúc viết, lập trình viên nghĩ rằng chúng sẽ được sử dụng nhiều lần, nhưng thực tế thì lại khác, nó chẳng được dùng mấy

### Loại bỏ các biến trung gian
![](https://i.imgur.com/di0hmzj.png)
```javascript=
var remove_one = function (array, value_to_remove) {
    var index_to_remove = null;
    for (var i = 0; i < array.length; i += 1) {
        if (array[i] === value_to_remove) {
            index_to_remove = i;
            break;
        }
    }
    if (index_to_remove !== null) {
        array.splice(index_to_remove, 1);
    }
};
```
Biến index_to_remove được sử dụng để lưu kết quả trung gian. Các biến như này thường thường sẽ được loại bỏ bằng việc xử lí kết quả ngay khi mà bạn nhận được, kiểu như:
```javascript=
var remove_one = function (array, value_to_remove) {
    for (var i = 0; i < array.length; i += 1) {
        if (array[i] === value_to_remove) {
            array.splice(i, 1);
            return;
        }
    }
};
```
Bằng việc xử lí splice và return luôn, chúng ta đã loại bỏ biến index_to_remove và đơn giản hóa mã nguồn.

Tổng kết, một chiến lược tốt là hoàn thành task càng nhanh càng tốt.

### Bỏ đi các biến điều khiển

Thỉnh thoảng bạn sẽ thấy thiết kế như này:

```javascript=
boolean done = false;
    while (/* condition */ && !done) {
    ...
    if (...) {
        done = true;
        continue;
    }
}
```

Biến done được set giá trị thành true rất nhiều lần ở trong vòng lặp.

Làm như vậy chỉ để đáp ứng một quy tắc bất thành văn là không bên thoát ra giữa vòng lặp. Không có quy tắc nào như thế cả!

Biến như done được gọi là "biến điều khiển." Mục đích của nó là chỉ đạo cách chương trình thực thi - nó không chứa bất kì dữ liệu thật sự nào cả. Với quan điểm của tôi, biến điều khiển nên bị loại bỏ bằng việc thiết kế cấu trúc chương trình tối ưu hơn:

```
while (/* condition */) {
    ...
    if (...) {
        break;
    }
}
```
Trường hợp này thì xử lí đơn giản rồi, nhưng với các vòng lặp lồng nhau mà chỉ với một lệnh break thì không đủ? Với trường hợp phức tạp thế, hướng giải quyết thường liên quan đến việc tách mã nguồn ra thành các hàm mới (hoặc là đoạn mã ở trong vòng lặp, hoặc là tổng thể cả vòng lặp)

> BẠN CÓ MUỐN ĐỒNG NGHIỆP CỦA MÌNH CẢM THẤY NHƯ LÀ
> HỌ LÚC NÀO CŨNG ĐANG Ở TRONG CUỘC PHỎNG VẤN?
> Eric Brechner của Microsoft đã nói rằng các câu hỏi phỏng vấn tuyệt vời nhất nên liên quan đến 3 biến trở lên.* Nó thật sự đúng bởi vì làm việc với 3 biến cùng một lúc sẽ gây ra khó khăn nhất định! Đấy là cảm giác cho một cuộc phỏng vấn, khi mà bạn cố gắng đẩy ứng viên đến giới hạn.
> Nhưng bạn muốn đồng nghiệp cảm thấy họ đang thực hiện một cuộc phỏng vấn khi mà họ đang đọc mã nguồn bạn viết ra à?

## Rút gọn phạm vi của các biến

Bạn đã từng nghe lời khuyên "tránh các biến toàn cục." bao giờ chưa. Đó là lời khuyên tốt đấy, bởi vì việc kiểm soát các biến này được sử dụng khi nào và như thế nào là tương đối khó khăn. Và vì sự rắc rối với tên biến (có thể nó sẽ trùng với biến địa phương), một cách vô tình, bạn đã thay đổi biến toàn cục khi mà ý định là làm với biến địa phương, hoặc ngược lại.

Thật sự, đó là một ý tưởng tuyệt vời khi mà "rút gọn phạm vi" cả tất cả các biến, không chỉ riêng với biến toàn cục

> KEY IDEA
> Làm cho biến xuất hiện càng ít dòng trên mã nguồn càng tốt

Rất nhiều ngôn ngữ lập trình cung cấp nhiều phạm vi/ cấp độ truy cập, ví dụ như module, class, function, và block. Sử dụng quyền truy cập hạn chế, nhìn tổng thể sẽ tốt hơn bởi vì nó nghĩa là biến của bạn có thể được "nhìn" thấy bởi ít dòng mã nguồn hơn.

Tại sao lại thế? Bởi vì nó làm giảm số lượng các biến mà người đọc phải nghĩ đến cùng một lúc một cách hiệu quả. Nếu bạn thu hẹp phạm vi của tất cả các biến theo hệ số hai, thì trung bình sẽ có một nửa số biến trong phạm vi bất kỳ lúc nào .

Ví dụ, giả sử bạn có một class rất lớn, với một biến mà được dùng bởi chỉ 2 methods, như cách dưới đây

```
class LargeClass {
    string str_;
    void Method1() {
        str_ = ...;
        Method2();
    }
    void Method2() {
        // Uses str_
    }
    // Lots of other methods that don't use str_ ...
};

```
Biến của class như thể  "biến cục bộ thu nhỏ" bên trong khu vực của class. Với các class lớn, thì việc quản lí các biến này và method nào ảnh hưởng đến biến nào là việc không dễ dàng. Biến cục bộ càng nhỏ, càng tốt.

Với thường hợp này, nên thu hẹp phạm vi của biến str_ thành biến địa phương:

```
class LargeClass {
    void Method1() {
        string str = ...;
        Method2(str);
    }
    void Method2(string str) {
        // Uses str
    }
    // Now other methods can't see str.
};
```

Cách khác để  hạn chế truy cập đến biến của class là tạo ra càng nhiều static methods càng tốt. Static methods là cách tốt nhất để người đọc biến "dòng mã nguồn nào được tách biệt khỏi các biến đó."

Hoặc một cách nữa là tách class thành các class nhỏ hơn. Hướng này chỉ có tác dụng nếu các lớp nhỏ hơn không ảnh hưởng qua lại lẫn nhau. Nếu bạn tạo ra 2 class mà truy cập đến nhau, thì thôi, bạn chưa đạt được mục tiêu của mình đâu.

Tương tự với việc chia nhỏ các file lớn thành các file nhỏ hơn hoặc các function lớn thành các function nhỏ hơn. Động lực lớn để làm như vậy là cô lập dữ liệu (tức là các biến).

Nhưng với các ngôn ngữ khác nhau, quy định cho phạm vi lại là khác nhau. Dưới đây, bạn sẽ thấy một số quy định thú vị liên quan đến phạm vi của biến.

### if Statement Scope in C++

Suppose you have the following C++ code:

```
PaymentInfo* info = database.ReadPaymentInfo();
    if (info) {
        cout << "User paid: " << info->amount() << endl;
    }
// Many more lines of code below ...
```
The variable info will remain in scope for the rest of the function, so the person reading this code might keep info in mind, wondering if/how it will be used again.

But in this case, info is only used inside the if statement. In C++, we can actually define info in the conditional expression:

```
if (PaymentInfo* info = database.ReadPaymentInfo()) {
    cout << "User paid: " << info->amount() << endl;
}
```
Now the reader can easily forget about info after it goes out of scope

## Creating “Private” Variables in JavaScript

Suppose you have a persistent variable that’s used by only one function:

```
submitted = false; // Note: global variable
var submit_form = function (form_name) {
    if (submitted) {
        return; // don't double-submit the form
    }
    ...
    submitted = true;
};
```

Global variables like submitted can cause the person reading this code a lot of angst. It seems like submit_form() is the only function that uses submitted, but you can’t know for sure. In fact, another JavaScript file might be using a global variable named submitted too, for a different purpose!

You can prevent this issue by wrapping submitted inside a closure:

```javascript=
var submit_form = (function () {
    var submitted = false; // Note: can only be accessed by the function below
    return function (form_name) {
        if (submitted) {
                return; // don't double-submit the form
            }
            ...
        submitted = true;
        };
}());

```
Note the parentheses on the last line—the anonymous outer function is immediately executed, returning the inner function.

If you haven’t seen this technique before, it may look strange at first. It has the effect of making a “private” scope that only the inner function can access. Now the reader doesn’t have to wonder Where else does submitted get used? or worry about conflicting with other globals of the same name. (See JavaScript: The Good Parts by Douglas Crockford [O’Reilly, 2008] for more techniques like this.)

### JavaScript Global Scope

In JavaScript, if you omit the keyword var from a variable definition (e.g., x = 1 instead of var x = 1), the variable is put into the global scope, where every JavaScript file and `<script>` block can access it. Here is an example:

```
<script>
var f = function () {
    // DANGER: 'i' is not declared with 'var'!
    for (i = 0; i < 10; i += 1) ...
};
f();
</script>
```

This code inadvertently puts i into the global scope, so a later block can still see it:
```
<script>
    alert(i); // Alerts '10'. 'i' is a global variable!
</script>
```

Many programmers aren’t aware of this scoping rule, and this surprising behavior can cause strange bugs. A common manifestation of this bug is when two functions both create a local variable with the same name, but forget to use var. These functions will unknowingly “cross-talk” and the poor programmer might conclude that his computer is possessed or that his RAM has gone bad.

The common “best practice” for JavaScript is to always define variables using the var keyword (e.g., var x = 1). This practice limits the scope of the variable to the (innermost) function where it’s defined.

## No Nested Scope in Python and JavaScript

Languages like C++ and Java have block scope, where variables defined inside an if, for, try, or similar structure are confined to the nested scope of that block:

```
if (...) {
    int x = 1;
}
x++; // Compile-error! 'x' is undefined.
```

But in Python and JavaScript, variables defined in a block “spill out” to the whole function. For example, notice the use of example_value in this perfectly valid Python code:

```
# No use of example_value up to this point.
if request:
    for value in request.values:
        if value > 0:
            example_value = value
            break
for logger in debug.loggers:
    logger.log("Example:", example_value)
```
This scoping rule is surprising to many programmers, and code like this is harder to read. In other languages, it would be easier to find where example_value was first defined—you would look along the “left-hand edge” of the function you’re inside.

The previous example is also buggy: if example_value is not set in the first part of the code, the second part will raise an exception: "NameError: ‘example_value’ is not defined". We can fix this, and make the code more readable, by defining example_value at the “closest common ancestor” (in terms of nesting) to where it’s used:

```python=
example_value = None

if request:
    for value in request.values:
        if value > 0:
            example_value = value
            break
if example_value:
    for logger in debug.loggers:
        logger.log("Example:", example_value)
```
However, this is a case where example_value can be eliminated altogether. example_value is just holding an intermediate result, and as we saw in “Eliminating Intermediate Results” on page 95, variables like these can often be eliminated by “completing the task as soon as possible.” In this case, that means logging the example value as soon as we find it.

Here’s what the new code looks like:

```python=
def LogExample(value):
    for logger in debug.loggers:
        logger.log("Example:", value)
    if request:
        for value in request.values:
            if value > 0:
                LogExample(value) # deal with 'value' immediately
                break
```

### Moving Definitions Down

The original C programming language required all variable definitions to be at the top of the function or block. This requirement was unfortunate, because for long functions with many variables, it forced the reader to think about all those variables right away, even if they weren't used until much later. (C99 and C++ removed this requirement.)

In the following example, all the variables are innocently defined at the top of the function:

```python
def ViewFilteredReplies(original_id):
    filtered_replies = []
    root_message = Messages.objects.get(original_id)
    all_replies = Messages.objects.select(root_id=original_id)
    root_message.view_count += 1
    root_message.last_view_time = datetime.datetime.now()
    root_message.save()

    for reply in all_replies:
        if reply.spam_votes <= MAX_SPAM_VOTES:
            filtered_replies.append(reply)
    return filtered_replies
```

The problem with this example code is that it forces the reader to think about three variables at once, and switch gears between them.

Because the reader doesn’t need to know about all of them until later, it’s easy to just move each definition right before its first use:
```
def ViewFilteredReplies(original_id):
    root_message = Messages.objects.get(original_id)
    root_message.view_count += 1
    root_message.last_view_time = datetime.datetime.now()
    root_message.save()

    all_replies = Messages.objects.select(root_id=original_id)
    filtered_replies = []
    for reply in all_replies:
        if reply.spam_votes <= MAX_SPAM_VOTES:
            filtered_replies.append(reply)
    return filtered_replies

```

You might be wondering whether all_replies is a necessary variable, or if it could be eliminated by doing:
```
for reply in Messages.objects.select(root_id=original_id):
...
```
In this case, all_replies is a pretty good explaining variable, so we decided to keep it.

## Prefer Write-Once Variables


![](https://i.imgur.com/DsykYv5.png)

So far in this chapter, we’ve discussed how it’s harder to understand programs with lots of variables “in play.” Well, it’s even harder to think about variables that are constantly changing.
Keeping track of their values adds an extra degree of difficulty.

To combat this problem, we have a suggestion that may sound a little strange: prefer writeonce variables.

Variables that are a “permanent fixture” are easier to think about. Certainly, constants like:
```
static const int NUM_THREADS = 10;
```

don’t require much thought on the reader’s part. And for the same reason, use of const in C++ (and final in Java) is highly encouraged.

In fact, in many languages (including Python and Java), some built-in types like string are immutable. As James Gosling (Java’s creator) said, “[Immutables] tend to more often be trouble free.”

But even if you can’t make your variable write-once, it still helps if the variable changes in fewer places.

> KEY IDEA
> The more places a variable is manipulated, the harder it is to reason about its current value.

So how do you do it? How can you change a variable to be write-once? Well, a lot of times it requires restructuring the code a bit, as you’ll see in the next example.

## A Finael Example

For the final example of the chapter, we’d like to show an example demonstrating many of the principles we’ve discussed so far.
Suppose you have a web page with a number of input text fields, arranged like this:

```htmlembedded=
<input type="text" id="input1" value="Dustin">
<input type="text" id="input2" value="Trevor">
<input type="text" id="input3" value="">
<input type="text" id="input4" value="Melissa">
...
```

As you can see, the ids start with input1 and increment from there.

Your job is to write a function named setFirstEmptyInput() that takes a string and puts it in the first empty <input> on the page (in the example shown, "input3"). The function should return the DOM element that was updated (or null if there were no empty inputs left). Here is some code to do this that doesn’t apply the principles in this chapter:

```javascript=
var setFirstEmptyInput = function (new_value) {
    var found = false;
    var i = 1;
    var elem = document.getElementById('input' + i);
    while (elem !== null) {
        if (elem.value === '') {
            found = true;
            break;
        }
        i++;
        elem = document.getElementById('input' + i);
    }
    if (found) elem.value = new_value;
    return elem;
};
```
This code gets the job done, but it’s not pretty. What’s wrong with it, and how do we improve it?
There are a lot of ways to think about improving this code, but we’re going to consider it from the perspective of the variables it uses:
- var found
- var i
- var elem


All three of these variables exist for the entire function and are written to multiple times. Let’s try to improve the use of each of them.

As we discussed earlier in the chapter, intermediate variables like found can often be eliminated by returning early. Here’s that improvement:

```
var setFirstEmptyInput = function (new_value) {
    var i = 1;
    var elem = document.getElementById('input' + i);
    while (elem !== null) {
        if (elem.value === '') {
            elem.value = new_value;
            return elem;
        }
        i++;
        elem = document.getElementById('input' + i);
    }
    return null;
};
```
Next, take a look at elem. It’s used multiple times throughout the code in a very “loopy” way where it’s hard to keep track of its value. The code makes it seem as if elem is the value we’re iterating through, when really we’re just incrementing through i. So let’s restructure the while loop into a for loop over i:

```
var setFirstEmptyInput = function (new_value) {
    for (var i = 1; true; i++) {
        var elem = document.getElementById('input' + i);
        if (elem === null)
            return null; // Search Failed. No empty input found.
        if (elem.value === '') {
            elem.value = new_value;
            return elem;
        }
    }
};

```

In particular, notice how elem acts as a write-once variable whose lifespan is contained inside the loop. The use of true as a for loop condition is unusual, but in exchange, we are able to see the definition and modifications of i in a single line. (A traditional while (true) would also be reasonable.)

## Summary
This chapter is about how the variables in a program can quickly accumulate and become too much to keep track of. You can make your code easier to read by having fewer variables and making them as “lightweight” as possible. Specifically:
- Eliminate variables that just get in the way. In particular, we showed a few examples of how to eliminate “intermediate result” variables by handling the result immediately.
- Reduce the scope of each variable to be as small as possible. Move each variable to a place where the fewest lines of code can see it. Out of sight is out of mind.
- Prefer write-once variables. Variables that are set only once (or const, final, or otherwise immutable) make code easier to understand.



























