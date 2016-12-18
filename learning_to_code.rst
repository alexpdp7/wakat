Learning to code
================

So you want to learn to program. It's a good profession; lots of work available, reasonably well paid and engaging. I have to say that it requires vocation to enjoy it and not be miserable at work, and to become good at it. But give it a try, even if you find out it's not something you want to do, you will learn interesting stuff and gain appreciation for it.

Learning to code is not easy. It is not something that you can watch/read lectures about and magically become able to do. It requires constant practice and challenge. You learn principally by:

* Knowing how it works. Computer programs are writting in programming languages and must follow the language's rules. No matter what, if your program does not comply, it will not work and you will not know if it's correct.
* Doing. You can master the rules of a language, but that's not enough to be able to code. You need to write programs and verify that they are correct.
* Watching others. Programming is complicated, and there are better and worse ways to do it. Watching how others have solved problems helps you become better faster; you do not need to figure on your own all solutions to all problems.

The most common question is... how to start?

My suggestion is to pick a project, an objective. I want to write an Asteroids clone. I want to create a weight-tracking website. Ideally, it should be something engaging to you, aligned with your interests. Maybe about some hobby you like, maybe something related to the work you are currently doing which would help you. It is useful that it is something that motivates you.

It is not always easy to find a good project; the most common problem is that you pick something which requires lots of effort to see results. When learning, it is very frustrating to have to work for weeks before seeing any result. Also, some tasks require more non-programming knowledge than others; a game such as Asteroids requires some degree of geometry/trigonometry knowledge. Building a website requires knowing HTML and how the internet works. Making an accounting program requires knowledge about financials, etc.

`Reddit <https://www.reddit.com/>`_'s `LEARNPROGRAMMING subreddit <https://www.reddit.com/r/learnprogramming/>`_ has `a good starting point for this <https://www.reddit.com/r/learnprogramming/wiki/faq#wiki_where_can_i_find_practice_exercises_and_project_ideas.3F>`_.

Once you pick your task, you should start working on it. One of the first tasks is picking a programming language. Many factors are in play here:

* First of all, the task itself might impose restrictions/recommendations. If you want to write an iPhone app, you will probably need to learn Swift. If you want to write a web application, it would be weird to use C++. Find about your domain and enumerate languages commonly used in that task.
* Preferrably, pick a language where you can find a willing tutor for. It helps to have someone you can ask questions about who is happy to help. It makes things much easier if they are familiar with the language you are using.
* Some languages are easier to learn than others. Notably, languages without memory management (principally, C and C++) are hard, as memory management is complicated even for experienced programmers. Some very theoretical languages such as Haskell tend to require complex concepts to be understood.
* Some languages have more abundance of teaching materials. Many universities, MOOCs, books, etc. use Python and Java in their lessons and thus there are more freely available teaching materials you can use.
* Some languages are more terrible than others. PHP and Javascript, while immensely popular, have serious drawbacks, both in general use and for learning.
* Statically-typed languages might be easier for beginners to work with, as they might prevent unexperienced programmers for making hard to debug mistakes. This is not a very objective matter, but I do think personally that it is true.
* Some languages are more marketable than others, and this might differ enormously between locations. Look for job postings in your area. Programming skills transfer very well from a language to other, so once you learn to code, learning a marketable language should be easy, but it might be interesting for your first language to be already useful to find a job.

For most purposes, I would choose Java or Python. Both are very popular, can be used for a wide variety of projects, and they have a wealth of enormous teaching materials. If you are very interested in iOS development, Swift might be a better choice. But if you say, have a Ruby fan willing to tutor you who dislikes Java, go for it!

Next, you should make yourself familiar with tutorials on your language- especially if you can find one adapted to your project (say, if you want to code a game in Python, you might be able to find a good tutorial which teaches you how to write some game from scratch).

Then, make a roadmap. The key here is to see results as soon as possible; if you are writing an Asteroids clone, showing a ship on the center on the screen is a great step. Then you can make it so that if you hit some specific keys, the ship rotates. Then add thrust and so on and so forth.

That is actually a very tough task- even if you are acquainted with programming, doubly so if you are just learning. Think about the easiest step you can. Can you break it down in smaller steps? Which knowledge outside of programming you need to implement it?

Then you need to set up your coding environment. Typically, each language requires something to run programs; for Java you need Oracle's JDK, Python provides an installer, etc. This step tends also to be complicated. Most tutorials will include an introduction that makes you write a very simple program and execute it.

To write your code, you will need a suitable code editor. Some languages have very clear choices here; if you are doing Swift development, Apple's Xcode is the obvious choice. Visual Studio is the de-facto choice for C# development, etc. However, for instance, Python and Java do not have "blessed" environments, and there is a wide choice available which can lead to paralysis.

Most programmers use either a "text editor" or an "IDE".

A text editor is a program exclusively designed to, well, edit text. Think of Microsoft Word, but without the formatting tools, ability to add images, etc. Code is mostly universally, plain text. Windows' Notepad is the simplest text editor you can find, but it has some issues which can be troublesome for a beginner.

The most popular editors out there are probably Emacs and vim. However, I would not recommend either of them as they have a steep learning curve (especially vim). Easier popular modern choices are Atom and Visual Studio Code, but many others exist. Try a few- editing is a big part of coding, and it's good to be comfortable with your editor- it should run fast and be responsive, you should find it easy on the eyes (many editors default to black text on a white screen, which some people find really uncomfortable- being able to switch easily to a "dark theme", that is, with a dark background, is a plus), etc.

An alternative is an IDE. IDE is short for Integrated Development Environment, and it aims to be more than a text editor. IDEs offer features such as being able to execute your programs without leaving the IDE, visual debugging (which lets you execute your programs step by step and analyze what they do, which many people learning to program find very useful), showing you errors in your code inline (just like some word processing software puts a wiggly underline on misspelled words, IDEs can do the same for some programming errors), show you related documentation to what you are doing inline, etc.

However, IDEs can be harder to set up, be heavier on resources and slower, and they can be confusing for people learning to program. Also, many text editors have plugins which grant them IDE-like features, so it's not really a clear distinction.

Spend some time with your tutorial's basic lesson on a starter program, trying with different editors/IDEs until you find something you are comfortable with.

Now, you can start working towards the first step in your project.

You should be aware of the existence of some disciplines which can help you:

* Testing. Obviously, one of the objectives when writing code is to write code which works correctly! Much has been done in the area of automated testing- creating a test suite for your code which checks that it is correct. For people learning to program, testing can provide tools which help you decompose your code in smaller units which you can develop and test in isolation, requiring less mental effort to achieve progress.
* Version control. Version control refers to being able to track the evolution of code. For people learning code, it principally gives you the ability to store the status of code when it's working so if you later modify it and "break it", you can easily go back to the working version.
