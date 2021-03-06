# Bạn đếch biết JS: Scope & Closures
# Chương 1: Scope (phạm vi) là gì?

Có một dạng chung cơ bản nhất của hầu hết tất cả ngôn ngữ lập trình đó là khả năng lưu trữ giá trị vào các biến, và sau đó là lấy ra hoặc sửa đổi các giá tri này. Trên thực tế, khả năng lưu trữ và lấy ra các giá trị từ các biến chính là thứ làm cho chương trình có được các *trạng thái*.

Nếu không có khả năng đó, chương trình chỉ có thể thực hiện được một vài tác vụ nhưng sẽ bị giới hạn rất nhiều và không có gì thú vị cả.

Nhưng việc đặt các biến vào chương trình lại làm nảy sinh một câu hỏi rất thú vị mà chúng ta sẽ đi tìm câu trả lời: các biến này *tồn tại* ở đâu? Hay nói một cách khác, chúng được lưu trữ ở đâu, và quan trọng nhât là làm thế nào để chương trình tìm được các biến đó khi cần?

Những câu hỏi này nói lên sự cần thiết về một bộ quy tắc chặt chẽ dùng cho việc lưu trữ các biến ở một vị trí nào đó và sau đó là cho việc tìm kiếm các biến này. Chúng ta gọi bộ quy tắc đó là *Scope*.

Nhưng những quy luật *Scope* này được thiết lập ở đâu và khi nào?

## Lý thuyết về trình biên dịch 

Tuỳ thuộc vào mức độ quen thuộc của bạn với các ngôn ngữ lập trình, bạn có thể cảm thấy hiển nhiên hoặc ngạc nhiên về điều này: mặc dù được xếp vào nhóm ngôn ngữ "động" hay "thông dịch" nhưng trên thực tế, JavaScript lại là một ngôn ngữ biên dịch.
pNó không được biên dịch kỹ ngay giống như nhiều ngôn ngữ biên dịch truyền thống, cũng không phải là mục đích cơ động, dùng để chạy được trên nhiều nền tảng khác nhau.

Tuy nhiên, bộ máy JavaScript thực hiện rất nhiều bước tương tự với các ngôn ngữ biên dịch khác theo những cách còn phức tạp hơn cả những điều mà ta thường thể nhận thấy.

Trong các bước của ngôn ngữ lập trình biên dịch truyền thống, một đoạn mã nguồn, tức là chương trình của bạn, sẽ đi qua 3 bước tiêu chuẩn như sau trước khi chúng được thực thi, thường được gọi là "quá trình biên dịch":

1. **Phân tích từ tố và từ vựng:** chia nhỏ chuỗi các kí tự thành các đoạn có nghĩa (đối với ngôn ngữ lập trình đó), được gọi là các từ tố (token). Ví dụ cụ thể, hãy xem đoạn chương trình: `var a = 2;`. Đoạn chương trình này sẽ được chia nhỏ thành các từ tố sau: `var`, `a`, `=`, `2` và `;`. Các khoảng trắng có thể được coi là các token hoặc không tùy theo trong ngôn ngữ đó, khoảng trắng đó có nghĩa hay không.

    **Ghi chú:** Sự khác nhau giữa phân tích từ tố và phân tích từ vựng mang tính chất hàn lâm, nhưng chủ yếu tập trung vào việc các token này *có trạng thái* hay *không có trạng thái*. Hiểu một cách đơn giản, nếu bộ phân tích từ vựng gọi bộ quy tắc có trạng thái ra để xem xét xem `a` có phải là một token độc lập hay chỉ là một phần của một token khác, thì đó được gọi là **lexing**.
    
2. **Phân tích:** lấy một chuỗi (một mảng) các token và chuyển chúng thành mô hình cây, cây này biểu diễn cho cấu trúc ngữ pháp cho chương trình. Cây này được gọi là "AST" (<b>A</b>bstract <b>S</b>yntax <b>T</b>ree - Cây ngữ pháp trừu tượng).


    Cây cho `var a = 2;` bắt đầu bằng một node cấp cao nhất được gọi là `Khai báo biến`, với một node con tên là `Định danh` (có giá trị là `a`), và một con khác là `Biểu thức gán`, bản thân con này lại có một con khác gọi là `Nguyên liệu kiểu số` (giá trị là `2`).

3. **Quá trình sinh mã:** là quá trình lấy một AST và chuyển nó thành chương trình có thể thực thi được. Phần này rất khác nhau và tùy thuộc vào ngôn ngữ lập trình, nền tảng môi trường mà chương trình sẽ được thực thi, v.v...

    Cho nên, thay vì sa lầy vào chi tiết, chúng ta sẽ chỉ lướt qua và ngầm định rằng có một cách nào đó để biến cây AST của đoạn `var a = 2;` thành mã máy để có thể thực sự *tạo ra* được một biến tên là `a` (bao gồm cả việc đảo bộ nhớ, v.v...), và sau đó là lưu 1 giá trị vào biến `a` đó.

    **Ghi chú:** Chi tiết về việc làm thế nào mà máy tính có thể quản lý các tài nguyên hệ thống sẽ nằm ngoài nội dung mà chúng ta sẽ tìm hiểu. Vì vậy chúng ta chấp nhận rằng máy tính sẽ có khả năng tạo và lưu trữ các biến khi cần.

Bộ máy JavaScript và các trình biên dịch của các ngôn ngữ lập trình khác trong thực tế hoạt động phức tạp hơn 3 bước này rất nhiều. Ví dụ, quá trình phân tích và sinh mã còn có thêm việc tối ưu hóa mã cho việc thực thi hiệu quả hơn, loại bỏ các thành phần bị dư thừa, v.v...

Vì thế, tôi chỉ đang vẽ bức tranh bằng những nét phác họa. Nhưng tôi nghĩ rằng bạn sẽ sớm nhận ra tại sao những chi tiết mà chúng ta đang xem xét, dù rằng ở mức độ sơ lược, cũng sẽ có liên quan.

Vì một điều, bộ thực thi JavaScript không có được một sự xa xỉ như các trình biên dịch khác, đó là khoảng thời gian đủ dài, dùng để tối ưu hóa mã chương trình, vì việc biên dịch của JavaScript không được thực hiện trong 1 quá trình build từ trước như các ngôn ngữ khác.

Đối với JavaScript, trong đa số trường hợp, việc biên dịch chỉ xảy ra trong một vài micro giây (hoặc ít hơn) ngay trước khi mã đó được thực thi. Để đảm bảo việc này được diễn ra nhanh nhất, máy thực thi JavaScript phải sử dụng đến khá nhiều kĩ xảo (như JITs: trì hoãn việc dịch hoặc thậm chí áp dụng "dịch nóng", v.v...), những kĩ xảo này cũng nằm xa ngoài phạm vi mà chúng ta sẽ tìm hiểu trong cuốn sách này.

Chúng ta hãy chỉ công nhận với nhau rằng, bất kì đoạn code JavaScript nào cũng được biên dịch trước khi (thường là *ngay* trước khi) nó được thực thi. Nên bộ biên dịch JS sẽ nhận vào đoạn code `var a = 2;` rồi dịch, và tạo ra các mã có thể thực thi ngay sau đó.

## Tìm hiểu về Scope (phạm vi)

Cách tốt nhất để học về scope là tưởng tượng về quá trình này như một đoạn hội thoại. Nhưng *ai* đang tham gia vào hội thoại này?

### Các vai diễn

Chúng ta hãy làm quen với các vai diễn của các nhân vật sẽ trao đổi với nhau trong quá trình xử lý đoạn chương trình `var a = 2;`, như thế chúng ta sẽ hiểu đoạn hội thoại mà lát nữa chúng ta sẽ theo dõi:

1. *Bộ máy (engine):* chịu trách nhiệm từ đầu đến cuối cho việc biên dịch và thực thi chương trình.

2. *Trình biên dịch (complier):* là một người bạn của *Engine*, xử lý tất cả các công việc phân tích và sinh mã (như đoạn trên).

3. *Scope*: là một người bạn khác của *Engine*, sưu tập và lưu giữ một danh sách tra cứu về tất cả các tên (biến) đã được khai báo, và nó đảm bảo việc thực thi về một bộ luật quy định về cách truy cập vào các biến này từ ngữ cảnh của đoạn code đang được thực thi.

Để bạn có thể *hiểu đầy đủ* về cách mà JS hoạt động, bạn cần phải *suy nghĩ* theo cách mà *Engine* (và những người bạn) nghĩ, phải hỏi câu hỏi như chúng hỏi, trả lời như chúng trả lời.

### Back & Forth

When you see the program `var a = 2;`, you most likely think of that as one statement. But that's not how our new friend *Engine* sees it. In fact, *Engine* sees two distinct statements, one which *Compiler* will handle during compilation, and one which *Engine* will handle during execution.

So, let's break down how *Engine* and friends will approach the program `var a = 2;`.

The first thing *Compiler* will do with this program is perform lexing to break it down into tokens, which it will then parse into a tree. But when *Compiler* gets to code-generation, it will treat this program somewhat differently than perhaps assumed.

A reasonable assumption would be that *Compiler* will produce code that could be summed up by this pseudo-code: "Allocate memory for a variable, label it `a`, then stick the value `2` into that variable." Unfortunately, that's not quite accurate.

*Compiler* will instead proceed as:

1. Encountering `var a`, *Compiler* asks *Scope* to see if a variable `a` already exists for that particular scope collection. If so, *Compiler* ignores this declaration and moves on. Otherwise, *Compiler* asks *Scope* to declare a new variable called `a` for that scope collection.

2. *Compiler* then produces code for *Engine* to later execute, to handle the `a = 2` assignment. The code *Engine* runs will first ask *Scope* if there is a variable called `a` accessible in the current scope collection. If so, *Engine* uses that variable. If not, *Engine* looks *elsewhere* (see nested *Scope* section below).

If *Engine* eventually finds a variable, it assigns the value `2` to it. If not, *Engine* will raise its hand and yell out an error!

To summarize: two distinct actions are taken for a variable assignment: First, *Compiler* declares a variable (if not previously declared in the current scope), and second, when executing, *Engine* looks up the variable in *Scope* and assigns to it, if found.

### Compiler Speak

We need a little bit more compiler terminology to proceed further with understanding.

When *Engine* executes the code that *Compiler* produced for step (2), it has to look-up the variable `a` to see if it has been declared, and this look-up is consulting *Scope*. But the type of look-up *Engine* performs affects the outcome of the look-up.

In our case, it is said that *Engine* would be performing an "LHS" look-up for the variable `a`. The other type of look-up is called "RHS".

I bet you can guess what the "L" and "R" mean. These terms stand for "Left-hand Side" and "Right-hand Side".

Side... of what? **Of an assignment operation.**

In other words, an LHS look-up is done when a variable appears on the left-hand side of an assignment operation, and an RHS look-up is done when a variable appears on the right-hand side of an assignment operation.

Actually, let's be a little more precise. An RHS look-up is indistinguishable, for our purposes, from simply a look-up of the value of some variable, whereas the LHS look-up is trying to find the variable container itself, so that it can assign. In this way, RHS doesn't *really* mean "right-hand side of an assignment" per se, it just, more accurately, means "not left-hand side".

Being slightly glib for a moment, you could also think "RHS" instead means "retrieve his/her source (value)", implying that RHS means "go get the value of...".

Let's dig into that deeper.

When I say:

```js
console.log( a );
```

The reference to `a` is an RHS reference, because nothing is being assigned to `a` here. Instead, we're looking-up to retrieve the value of `a`, so that the value can be passed to `console.log(..)`.

By contrast:

```js
a = 2;
```

The reference to `a` here is an LHS reference, because we don't actually care what the current value is, we simply want to find the variable as a target for the `= 2` assignment operation.

**Note:** LHS and RHS meaning "left/right-hand side of an assignment" doesn't necessarily literally mean "left/right side of the `=` assignment operator". There are several other ways that assignments happen, and so it's better to conceptually think about it as: "who's the target of the assignment (LHS)" and "who's the source of the assignment (RHS)".

Consider this program, which has both LHS and RHS references:

```js
function foo(a) {
	console.log( a ); // 2
}

foo( 2 );
```

The last line that invokes `foo(..)` as a function call requires an RHS reference to `foo`, meaning, "go look-up the value of `foo`, and give it to me." Moreover, `(..)` means the value of `foo` should be executed, so it'd better actually be a function!

There's a subtle but important assignment here. **Did you spot it?**

You may have missed the implied `a = 2` in this code snippet. It happens when the value `2` is passed as an argument to the `foo(..)` function, in which case the `2` value is **assigned** to the parameter `a`. To (implicitly) assign to parameter `a`, an LHS look-up is performed.

There's also an RHS reference for the value of `a`, and that resulting value is passed to `console.log(..)`. `console.log(..)` needs a reference to execute. It's an RHS look-up for the `console` object, then a property-resolution occurs to see if it has a method called `log`.

Finally, we can conceptualize that there's an LHS/RHS exchange of passing the value `2` (by way of variable `a`'s RHS look-up) into `log(..)`. Inside of the native implementation of `log(..)`, we can assume it has parameters, the first of which (perhaps called `arg1`) has an LHS reference look-up, before assigning `2` to it.

**Note:** You might be tempted to conceptualize the function declaration `function foo(a) {...` as a normal variable declaration and assignment, such as `var foo` and `foo = function(a){...`. In so doing, it would be tempting to think of this function declaration as involving an LHS look-up.

However, the subtle but important difference is that *Compiler* handles both the declaration and the value definition during code-generation, such that when *Engine* is executing code, there's no processing necessary to "assign" a function value to `foo`. Thus, it's not really appropriate to think of a function declaration as an LHS look-up assignment in the way we're discussing them here.

### Engine/Scope Conversation

```js
function foo(a) {
	console.log( a ); // 2
}

foo( 2 );
```

Let's imagine the above exchange (which processes this code snippet) as a conversation. The conversation would go a little something like this:

> ***Engine***: Hey *Scope*, I have an RHS reference for `foo`. Ever heard of it?

> ***Scope***: Why yes, I have. *Compiler* declared it just a second ago. He's a function. Here you go.

> ***Engine***: Great, thanks! OK, I'm executing `foo`.

> ***Engine***: Hey, *Scope*, I've got an LHS reference for `a`, ever heard of it?

> ***Scope***: Why yes, I have. *Compiler* declared it as a formal parameter to `foo` just recently. Here you go.

> ***Engine***: Helpful as always, *Scope*. Thanks again. Now, time to assign `2` to `a`.

> ***Engine***: Hey, *Scope*, sorry to bother you again. I need an RHS look-up for `console`. Ever heard of it?

> ***Scope***: No problem, *Engine*, this is what I do all day. Yes, I've got `console`. He's built-in. Here ya go.

> ***Engine***: Perfect. Looking up `log(..)`. OK, great, it's a function.

> ***Engine***: Yo, *Scope*. Can you help me out with an RHS reference to `a`. I think I remember it, but just want to double-check.

> ***Scope***: You're right, *Engine*. Same guy, hasn't changed. Here ya go.

> ***Engine***: Cool. Passing the value of `a`, which is `2`, into `log(..)`.

> ...

### Quiz

Check your understanding so far. Make sure to play the part of *Engine* and have a "conversation" with the *Scope*:

```js
function foo(a) {
	var b = a;
	return a + b;
}

var c = foo( 2 );
```

1. Identify all the LHS look-ups (there are 3!).

2. Identify all the RHS look-ups (there are 4!).

**Note:** See the chapter review for the quiz answers!

## Nested Scope

We said that *Scope* is a set of rules for looking up variables by their identifier name. There's usually more than one *Scope* to consider, however.

Just as a block or function is nested inside another block or function, scopes are nested inside other scopes. So, if a variable cannot be found in the immediate scope, *Engine* consults the next outer containing scope, continuing until found or until the outermost (aka, global) scope has been reached.

Consider:

```js
function foo(a) {
	console.log( a + b );
}

var b = 2;

foo( 2 ); // 4
```

The RHS reference for `b` cannot be resolved inside the function `foo`, but it can be resolved in the *Scope* surrounding it (in this case, the global).

So, revisiting the conversations between *Engine* and *Scope*, we'd overhear:

> ***Engine***: "Hey, *Scope* of `foo`, ever heard of `b`? Got an RHS reference for it."

> ***Scope***: "Nope, never heard of it. Go fish."

> ***Engine***: "Hey, *Scope* outside of `foo`, oh you're the global *Scope*, ok cool. Ever heard of `b`? Got an RHS reference for it."

> ***Scope***: "Yep, sure have. Here ya go."

The simple rules for traversing nested *Scope*: *Engine* starts at the currently executing *Scope*, looks for the variable there, then if not found, keeps going up one level, and so on. If the outermost global scope is reached, the search stops, whether it finds the variable or not.

### Building on Metaphors

To visualize the process of nested *Scope* resolution, I want you to think of this tall building.

<img src="fig1.png" width="250">

The building represents our program's nested *Scope* rule set. The first floor of the building represents your currently executing *Scope*, wherever you are. The top level of the building is the global *Scope*.

You resolve LHS and RHS references by looking on your current floor, and if you don't find it, taking the elevator to the next floor, looking there, then the next, and so on. Once you get to the top floor (the global *Scope*), you either find what you're looking for, or you don't. But you have to stop regardless.

## Errors

Why does it matter whether we call it LHS or RHS?

Because these two types of look-ups behave differently in the circumstance where the variable has not yet been declared (is not found in any consulted *Scope*).

Consider:

```js
function foo(a) {
	console.log( a + b );
	b = a;
}

foo( 2 );
```

When the RHS look-up occurs for `b` the first time, it will not be found. This is said to be an "undeclared" variable, because it is not found in the scope.

If an RHS look-up fails to ever find a variable, anywhere in the nested *Scope*s, this results in a `ReferenceError` being thrown by the *Engine*. It's important to note that the error is of the type `ReferenceError`.

By contrast, if the *Engine* is performing an LHS look-up and arrives at the top floor (global *Scope*) without finding it, and if the program is not running in "Strict Mode" [^note-strictmode], then the global *Scope* will create a new variable of that name **in the global scope**, and hand it back to *Engine*.

*"No, there wasn't one before, but I was helpful and created one for you."*

"Strict Mode" [^note-strictmode], which was added in ES5, has a number of different behaviors from normal/relaxed/lazy mode. One such behavior is that it disallows the automatic/implicit global variable creation. In that case, there would be no global *Scope*'d variable to hand back from an LHS look-up, and *Engine* would throw a `ReferenceError` similarly to the RHS case.

Now, if a variable is found for an RHS look-up, but you try to do something with its value that is impossible, such as trying to execute-as-function a non-function value, or reference a property on a `null` or `undefined` value, then *Engine* throws a different kind of error, called a `TypeError`.

`ReferenceError` is *Scope* resolution-failure related, whereas `TypeError` implies that *Scope* resolution was successful, but that there was an illegal/impossible action attempted against the result.

## Review (TL;DR)

Scope is the set of rules that determines where and how a variable (identifier) can be looked-up. This look-up may be for the purposes of assigning to the variable, which is an LHS (left-hand-side) reference, or it may be for the purposes of retrieving its value, which is an RHS (right-hand-side) reference.

LHS references result from assignment operations. *Scope*-related assignments can occur either with the `=` operator or by passing arguments to (assign to) function parameters.

The JavaScript *Engine* first compiles code before it executes, and in so doing, it splits up statements like `var a = 2;` into two separate steps:

1. First, `var a` to declare it in that *Scope*. This is performed at the beginning, before code execution.

2. Later, `a = 2` to look up the variable (LHS reference) and assign to it if found.

Both LHS and RHS reference look-ups start at the currently executing *Scope*, and if need be (that is, they don't find what they're looking for there), they work their way up the nested *Scope*, one scope (floor) at a time, looking for the identifier, until they get to the global (top floor) and stop, and either find it, or don't.

Unfulfilled RHS references result in `ReferenceError`s being thrown. Unfulfilled LHS references result in an automatic, implicitly-created global of that name (if not in "Strict Mode" [^note-strictmode]), or a `ReferenceError` (if in "Strict Mode" [^note-strictmode]).

### Quiz Answers

```js
function foo(a) {
	var b = a;
	return a + b;
}

var c = foo( 2 );
```

1. Identify all the LHS look-ups (there are 3!).

	**`c = ..`, `a = 2` (implicit param assignment) and `b = ..`**

2. Identify all the RHS look-ups (there are 4!).

    **`foo(2..`, `= a;`, `a + ..` and `.. + b`**


[^note-strictmode]: MDN: [Strict Mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions_and_function_scope/Strict_mode)
