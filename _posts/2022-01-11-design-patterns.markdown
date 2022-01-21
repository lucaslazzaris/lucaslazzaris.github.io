---
layout: post
title:  "GoF Design Patterns"
date:   2022-01-11 21:19:17 -0300
smmry: I decided to code all the examples from the famous book "Design patterns elements of reusable object-oriented software" using go.
read_time: 3 min
---

Are you thinking about reading and implementing GoF Design Patterns? Here's what I have to tell you.

I've first heard about the Gang of Four's (GoF) book was on a podcast. That was before I started working as a software engineer. So my understanding of that podcast was about zero.

The book is famous because it is a source for object-oriented design, like many modern languages. And as a bonus, it's well written. It employs two different programming languages: mainly C++ and some Smalltalk. The book has a lot to offer, even for those who don't work using C++ or Smalltalk.

It presents 23 software patterns, although “Singleton” is considered an anti-pattern today. The patterns are divided into three groups based on their most promising characteristic: creational, behavioral or structural. As an OOP book, there is a lot of polymorphism, interfacing, inheritance, making C++ or Java great candidates to code the book's examples.

However, I worked for some years using C++ and, I don't really want to learn Java now. So I chose to implement the patterns using a language I'd like to learn: Go. I made my choice before I knew the first thing about Go.

Go is a statically typed, compiled programming language. It seems like a great language to do parallel programming. It has structs similar to classes, but it doesn't support inheritance. As you might guess, coding an OOP design pattern using a language without inheritance may present some obstacles.

## What I learned

I've never written code with interfaces before (yep, no Typescript/Java). It was enlightening to define two different structs that shared the same interface and could be passed as an argument to a function. I hope to work again with interfaces.

I learned Go looking at the examples from [Go by Example](https://gobyexample.com/) and [A Tour of Go](https://go.dev/tour/welcome/1). I used [Learn Go with Tests](https://github.com/quii/learn-go-with-tests) as the principal reference for the testing API.

Testing using Go is not as hard as I thought at first. The first time I saw code like the one below, I got the feeling that something was gonna break. Probably there is some framework to do it in a less verbose way, but my goal was to use the “testing” module.

```go
import "testing"

func TestFunc(t *testing.T) { 
	expected := true
	actual := func()
	if actual != expected {
		 t.Errorf("Actual: %t Expected: %t", actual, expected)
	}
}
```

I read the book after I started working as a software engineer. I was thrilled when I realized I intuitively used some patterns without knowing. And looking at the codebase from where I work, there are great uses of design patterns. I wouldn't say you'll be able to use all the 23 patterns every day. But this book will add some new tools to your programming skills.


I recommend this book for people who already understand the basics of OOP. If you are starting to code, first get the basics of your programming language, then your field (like an MVC framework for web developers), and, finally, the design patterns.


If you are interested, here is the [GitHub Repo](https://github.com/lucaslazzaris/design_patters_go). Or if you have some feedback, please contact me: <a href="mailto:lucascercal.l@gmail.com">lucascercal.l@gmail.com</a>.