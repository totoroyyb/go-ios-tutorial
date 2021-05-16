---
layout: page
title: Use Button
description: >
  In this post, you will learn some basic usage of `Button`. Beyond that, you will also start to learn more about `@State`.
hide_description: false
sitemap: false
---

1. this unordered seed list will be replaced by toc as unordered list
{:toc}

## Create a `Button`!

Let's open up `ContentView.swfit` files. Clean up all codes, and create a `VStack` containing a `Text` and a `Button`. Like this:

``` swift
// file: 'ContentView.swift'
...
struct ContentView: View {
    var body: some View {
        VStack {
            Text("Hello, world")
            Button("Click Me") {
                print("Clicked.")
            }
        }
    }
}
...
```

After that, you will see the preview be like this:

![rect](intro/../../assets/img/intro/4/1.png)

You can see the `Text` and `Button` already appeared on the screen. The `Button` is labelled as "Click Me" and the default `foreground` color is blue. Let's take a close look at the code above, to see how I declare this `Button`.

Now, let's discuss a little about how the SwiftUI view is made. You already know how we declare a SwiftUI view --- using a struct. Yes, every SwiftUI view indeed is a struct! So we declare the view just how we declare the struct. Taking `Text` as an example, `Text`, as a struct, has a initializer (constructor in Swift term) which takes in a `String` as parameter.

> Note: `Text` actually takes in a `StringProtocol`, which is not quite same as taking `String`. What I mentioned above is just for a simple explanation in case you don't understand the difference between these two.

Now, check out the declaration of `Button`. Probably this form of declaration might be more familiar to you:

``` swift
Button("Click Me", action: { print("Clicked.") })
```

In this case, `Button` takes into two parameters, one is the text string which is the displayed content of this button, and another one is the `action`. This `action` parameter is closure with type `() -> Void`. This parameter defines what to do when users tap on the button.

If you have previous knowledge about Swift and clousure, it should be really esay to understand the transformation between these two ways of declarations. If not, check out this [document](https://docs.swift.org/swift-book/LanguageGuide/Closures.html#ID97) from Apple.

Now back to the cleaner way of declaration:

``` swift
// file: 'ContentView.swift'
...
struct ContentView: View {
    var body: some View {
        VStack {
            Text("Hello, world")
            Button("Click Me") {
                print("Clicked.")
            }
        }
    }
}
...
```

In this view, when user pressed the `Button`, "Clicked." shall be printed on the console. You can try out by running this. You can do that in the preview panel or in a separate simulator. *If you want to do in preview panel, you need to press the "run" button as indicated in the previous image.*

## How to change the `Text` when press the `Button`?

Creating a button is pretty straightforward right? Then, what if we want more? For example, changing the text when press the `Button`.

Let's do something like this:

``` swift
// file: 'ContentView.swift'
...
struct ContentView: View {
    var text = "Hello, world"
    
    var body: some View {
        VStack {
            Text(text)
            Button("Click Me") {
                text = "Changed!"
            }
        }
    }
}
...
```

The idea is simple, create a `var` (variable) and let the text display this string. Also, change the value of this variable to something else. BUT, Xcode panics! Xcode gives out an error --- "Cannot assign to property: 'self' is immutable".

Why is that? That's because in Swift struct is immutable and passed by value, instead of reference. Once the struct is created, all its properties cannot be modified, even if you are marked as `var` instead of `let`. In this case, we need a special thing called "Property Wrapper" --- `@State`.

> If you want to know more about how "Property Wrapper" works, you can check out this [document](https://docs.swift.org/swift-book/LanguageGuide/Properties.html) from Apple.

## Introducing `@State`!

Now let's mark the `var text` with `@State` like this:

``` swift
@State var text = "Hello, world"
```

Give it another try! You shall see how it's working. There must be something magic happening! `@State` changed a immutable object? No, no, no. No magic at all. `struct` in swift is immuatable, ALWAYS! However, you can make a new copy of it with some different property initializations, right? This is what SwiftUI do behind the scene, or shall say, what `@State` do behind the scene.

Every time when your codes modify a variable marked as `@State`, it will trigger a rerender of UI elements. This might bring up another issue --- efficiency. If it requires to redraw entire UI elements each time when the state changes, and the copying the struct again and again can be costly. Generally, this is correct. However, SwiftUI managed to achieve an efficient copy in most situation with a technique named as `diffing` algorithm. With the help of a well-implemented `diffing` algorithm, SwiftUI can identify which element is identical and which one needs to update. However, it is also very possible that you might use your own `diffing` algorithm under some circumstance. More details about `diffing` might be discussed in the future posts.

## `if-else` in SwiftUI view

Above example demonstrates how to change the text of one `Text` view. Now, we can try to make two `Text` view with different text and dynamically switch between these two views. Modify your code to this:

``` swift
// file: 'ContentView.swift'
...
struct ContentView: View {
    var text1 = "Hello"
    var text2 = "World"
    
    @State var isOn = true
    
    var body: some View {
        VStack {
            if isOn {
                Text(text1)
            } else {
                Text(text2)
            }
            
            Button("Click Me") {
                isOn.toggle()
            }
        }
    }
}
...
```

Try to run it and test it out. This time we utilize a `if-else` statement. Recall all SwiftUI components are structs. What it takes are parameters of struct initializer. So for the content of a SwiftUI view it takes a function or closure. In this case, using a `if-else` statement becomes totally valid and logical.

## Ending

That all for this post. Let's do some practice about how to use `@State` and do a recap about how the SwiftUI manage this behind the scene.

<!-- 
Continue with [Config](config.md){:.heading.flip-title}
{:.read-more} -->
