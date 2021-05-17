---
layout: page
title: Basic Animation
description: >
  In this post, you will learn how to apply basic animations.
hide_description: false
sitemap: false
---

1. this unordered seed list will be replaced by toc as unordered list
{:toc}

## Let's setup our view by creating a `Shape`!

Let's open up `ContentView.swfit` files. Replaces all `Text` views with a `Rectangle` declaration. Like this:

``` swift
// file: 'ContentView.swift'
...
struct ContentView: View {
    var body: some View {
        Rectangle()
            .frame(width: 100, height: 100)
    }
}
...
```

## Let's try to move this `Rectangle`!

How to move a view around? Recall from previous posts, there is one `ViewModifier` named `.offset()`. This time, create two `@State` variables, and set the `width` and `height` of these two variables by using the new declared `@State` variables, like this:

``` swift
// file: 'ContentView.swift'
...
struct ContentView: View {
    @State var xOffset: CGFloat = 0
    @State var yOffset: CGFloat = 0
    
    var body: some View {
        Rectangle()
            .frame(width: 100, height: 100)
            .offset(x: xOffset, y: yOffset)
    }
}
...
```

At this point, we can create a `Button` to trigger the action of moving this `Rectangle`. Like this:

``` swift
// file: 'ContentView.swift'
...
var body: some View {
  VStack {
      Rectangle()
          .frame(width: 100, height: 100)
          .offset(x: xOffset, y: yOffset)
      
      Button("Click to move.") {
          xOffset = xOffset == 0 ? -100 : 0
          yOffset = yOffset == 0 ? -100 : 0
      }
  }
}
...
```

> In case you don't understand the expression of `ternary operator`, here are some basic information. `xOffset == 0 ? -100 : 0` uses a ternary operator. It evaluates `(xOffset == 0)` first to check whether this is true or false. 
> - If it is true, then it returns the evaluation of the expression right after `?` but before `:`, which is `-100` in this case. 
> - If it is false, then it returns the evaluation of the expression after `:`, which is `0` in this case.

After correctly setting up the view, run it and try it out. You should see the rectangle moves when clicking the button. But it lacks animation!

## How to apply animation to the view?

There are two major ways to animate the view, let's go one by one here.

### Introducing `.animation()` ViewModifier!

Add `.animation()` ViewModifier to the view that you want to animate, then you will have a default animation. Try this:

``` swift
// file: 'ContentView.swift'
...
Rectangle()
    .frame(width: 100, height: 100)
    .offset(x: xOffset, y: yOffset)
    .animation(.default)
...
```

Now, rerun the preview, and you shall be able to see the `Rectangle` animates in a linear way, which is indicated by `Animation.default`. For more advance animations, you can change the parameter passed into. For example, you can use `Animation.easeIn`, `Animation.easeOut`, `Animation.spring`, and so on.

> When the Swift compiler can automatically infer the type, you can omit some typing. For example, when Swift compiler knows you will pass in a struct with type `Animation`, you can omit `Animation` part and simply put down `.spring`, for example.

### Introducing `withAnimation()`!

There is another way of doing animation, by using `withAnimation()` function. Now, let's remove what we did with `.animation` ViewModifier, and do this for the `Button`:

``` swift
// file: 'ContentView.swift'
...
Button("Click to move.") {
    withAnimation(.default) {
        xOffset = xOffset == 0 ? -100 : 0
        yOffset = yOffset == 0 ? -100 : 0
    }
}
...
```

You will see it has the same effect as the previous way. However, they work in a different way and sometimes you might need to explicitly choose one over another.

`.animation` will be automatically applied to the target view's children views. For example, you can do this:

``` swift
// file: 'ContentView.swift'
...
var body: some View {
  VStack {
      Rectangle()
          .frame(width: 100, height: 100)
          .offset(x: xOffset, y: yOffset)
          
      
      Button("Click to move.") {
          xOffset = xOffset == 0 ? -100 : 0
          yOffset = yOffset == 0 ? -100 : 0
      }
      .offset(x: xOffset, y: yOffset)
  }
  .animation(.default)
}
...
```

In this case, you can only apply one `.animation()` to the `VStack` to get both `Rectangle` and `Button` animated.

However, `withAnimation` will only animate specified views. Taking what we did above to the `Button` as an example, `withAnimation` will see we want to animate `xOffset` and `yOffset`. Hence, when the SwiftUI doing the rendering, it will only animate the specific views which associated with the target variables. In this case, `Rectangle` is associated with both `xOffset` and `yOffset`.


## Ending

This is a basic introduction of the animation in SwiftUI. More advanced topics might be discussed in the following posts. You can tweak around the parameter of both `.animation` or `withAnimation` to achieve different animation style.

<!-- Continue with [Use Button](button.md){:.heading.flip-title}
{:.read-more} -->
