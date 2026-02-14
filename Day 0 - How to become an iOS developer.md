**Course:** 100 Days of SwiftUI
**Date:** February 14, 2026
**Status:** Notes completed

> **Quick Summary:** A complete roadmap for becoming an iOS developer, covering the essential skills you actually need to get hired (spoiler: it's only 5 core skills), common mistakes that slow people down, the best free resources to learn from, and how to connect with the community. No fluff, no gatekeeping—just a practical path from zero to job-ready.

**Source:** [Original article](https://www.hackingwithswift.com/articles/230/how-to-become-an-ios-developer) | [Video version](https://youtu.be/HNXzcAwNqMc)

---

## Table of Contents

1. [Core Skills](https://claude.ai/chat/f8e42c05-3b11-4ba1-a3fc-c9807aa3f330#1-core-skills)
2. [Extension Skills](https://claude.ai/chat/f8e42c05-3b11-4ba1-a3fc-c9807aa3f330#2-extension-skills)
3. [Common Mistakes](https://claude.ai/chat/f8e42c05-3b11-4ba1-a3fc-c9807aa3f330#3-common-mistakes)
4. [Learning Resources](https://claude.ai/chat/f8e42c05-3b11-4ba1-a3fc-c9807aa3f330#4-learning-resources)
5. [Connecting to the Community](https://claude.ai/chat/f8e42c05-3b11-4ba1-a3fc-c9807aa3f330#5-connecting-to-the-community)
6. [How Long It Will Take](https://claude.ai/chat/f8e42c05-3b11-4ba1-a3fc-c9807aa3f330#6-how-long-it-will-take)
7. [Preparing to Apply for a Job](https://claude.ai/chat/f8e42c05-3b11-4ba1-a3fc-c9807aa3f330#7-preparing-to-apply-for-a-job)

---

## Skills Overview: What You Actually Need

|Skill Type|What's Included|When to Learn|
|---|---|---|
|**Core Skills** (Must-have)|Swift, SwiftUI, Networking, Data, Git|Start here—this is enough to get hired|
|**Extension Skills** (Level-up)|UIKit, Core Data, Testing, Architecture, Multithreading|After you're comfortable with the core|

The philosophy here is refreshingly simple: master the core skills first, build real apps (even if your code is messy), and learn the rest on the job. You don't need to know everything before you start—in fact, trying to learn it all upfront will just slow you down.

---

## 1. Core Skills

These are the five non-negotiable skills you need to land your first iOS job. That's it. Not fifteen, not twenty—five.

### 1.1 Learning Swift

Swift is Apple's core programming language. Think of it as the raw material—variables, functions, loops, logic—before you start building interfaces or connecting to servers.

It's a modern language packed with powerful features that can feel overwhelming at first. The good news? You don't need to master everything immediately. Many parts are straightforward, and the complex stuff will click with practice and repetition.

Swift is the foundation. Without it, nothing else works.

### 1.2 Learning SwiftUI

SwiftUI is Apple's modern framework for building user interfaces across iOS, macOS, tvOS, and watchOS. While Swift is the language, SwiftUI gives you the tools—buttons, text fields, images, navigation—to actually build what users see and interact with.

**Why SwiftUI over UIKit?**

- Way less code for the same result
- Automatically updates your UI when data changes
- Works across all Apple platforms
- Significantly easier to learn
- Represents the future of Apple development

Even though UIKit is still common in existing apps, SwiftUI is the smarter choice for learning. You can always pick up UIKit later if a job requires it.

### 1.3 Networking and Data

These two skills go hand in hand: fetching data from a server (networking) and turning that data into something your app can use (data handling).

At a junior level, this mostly means:

- Making HTTP requests to APIs
- Parsing JSON responses
- Converting that data into Swift structures your app understands

This is a huge part of real-world iOS development. Almost every app needs to talk to a server and handle the data it gets back.

### 1.4 Version Control (Git)

You don't need to be a Git expert, but you do need to know enough to:

- Save your code safely
- Track changes over time
- Publish your projects on GitHub

Why? Because recruiters want to see your code. GitHub is your portfolio. If you can push code to a repo and navigate basic version control, you're good to go.

---

**That's it. Five core skills.** Master these, and you're qualified to build apps, work as an indie developer, or apply for junior iOS roles. No certifications required, no CS degree necessary—just these five skills and the projects you build with them.

---

## 2. Extension Skills

Once you've nailed the core five, these extension skills will level you up from "qualified" to "competitive." They're not required to get started, but they'll expand what you can build and make you more attractive to employers.

### 2.1 UIKit

UIKit is Apple's older UI framework, introduced in 2008. It's powerful, mature, and runs in hundreds of thousands of existing apps—which means if you work at a company with an established codebase, you'll probably encounter it.

**The trade-offs:**

- **Pros:** More precise control, vast ecosystem of solutions, essential for maintaining legacy projects
- **Cons:** Way more complex than SwiftUI, requires tons more code, built for Objective-C so it feels clunky in Swift

UIKit is worth learning eventually, but it takes serious time and patience. Start with SwiftUI, then circle back to UIKit when you're ready for the challenge.

### 2.2 Core Data

Core Data is Apple's framework for managing and persisting data locally. While basic data handling lets you fetch and display JSON, Core Data lets you:

- Store large amounts of data permanently on the device
- Search, sort, and filter efficiently
- Create relationships between data (like users and their posts)
- Sync data across devices using iCloud

Like UIKit, Core Data shows its age. It was designed with Objective-C in mind and can feel awkward in Swift, even with SwiftUI improvements.

It's powerful and widely used in production apps, but you don't need it to build great apps. Learn it when you're ready to go beyond the basics.

### 2.3 Testing

Testing means writing code that verifies your app behaves correctly. It's crucial for:

- Catching bugs before users do
- Confidently refactoring code
- Ensuring new features don't break existing ones

**So why is it an extension skill?** Three reasons:

1. The iOS community historically hasn't emphasized testing as much as other platforms
2. Many developers find it less exciting than building features
3. When job hunting, strong knowledge of Swift and SwiftUI carries more weight than testing expertise

Testing absolutely matters for professional development, but it's best learned after you're comfortable building and shipping apps.

### 2.4 Software Architecture

Software architecture is about organizing your code so it's readable, maintainable, and easy to modify later.

Early on, your code will be messy. That's completely normal. But as you learn design patterns and proven techniques, you'll start structuring apps into clean, reusable components—separating concerns like login logic, image galleries, and friend lists into distinct, manageable parts.

**Here's the thing:** architecture is subjective. There's rarely one "right" way to structure an app. The best measure of progress is looking back at old code and cringing at how bad it was. That cringe means you're improving.

### 2.5 Multithreading

Multithreading means making your app do multiple things simultaneously—like downloading an image while the user scrolls through a list.

It's powerful but tricky. Parallel code can be hard to reason about, and adding concurrency doesn't always make things faster—it can even slow things down while making your code more complex.

The goal here isn't to become a concurrency expert. It's to understand enough to use it safely and correctly in Swift without overcomplicating your apps.

---

## 3. Common Mistakes

These are the seven traps that slow down most learners. Avoid them, and you'll progress much faster.

### 3.1 Trying to Memorize Everything

No one can remember every API, method, or syntax detail. There are thousands of things to learn, and trying to memorize them all will drain your energy and kill your motivation.

**The truth:** Learning happens through repetition. You try something, forget it, look it up again, and gradually the core concepts stick. Forgetting is actually part of the learning process—each time you relearn something, your brain strengthens those neural pathways.

The skill you need isn't memorization. It's knowing where to find information and trusting that repeated practice will naturally embed the knowledge.

### 3.2 Shiny Object Syndrome

This is when you jump from tutorial to tutorial, course to course, whenever things get tough or boring.

The problem? Real learning involves parts that aren't immediately rewarding. By constantly switching to something new, you stay stuck in the easy, shallow parts instead of pushing through to master the harder concepts.

You don't have to be perfect about this. Just be aware: when you hit a challenging section, seek help or push through rather than abandoning ship. Eventually, you'll need to learn those concepts anyway—might as well do it now.

### 3.3 Lone Wolf Learning

Trying to learn iOS development completely on your own is a recipe for frustration. Sure, a few experienced programmers can pull it off, but most people struggle: mistakes take forever to fix, motivation tanks, and you miss out on the inspiration that comes from seeing what others are building.

**The fix:** Connect with fellow learners. Share your progress, ask questions, engage with the community. You'll gain support, encouragement, and inspiration—and you'll learn way faster while enjoying the process more.

### 3.4 Using Beta Software

It's tempting to download the latest beta of iOS or Xcode to try out new features. Don't do it—at least not while you're learning.

Betas cause problems:

- Tutorials won't match your setup
- Bugs are common and frustrating
- APIs change between versions, breaking your code

Stick with the latest public releases until you're confident and comfortable with the tools. Save the beta experimentation for when you know what you're doing.

### 3.5 Relying on Apple's Documentation

Apple's documentation is thorough and comprehensive. It's also a terrible way to learn iOS development from scratch.

Think of it like trying to learn a language by reading a dictionary. Apple's guides are reference material—great for looking up specifics, but not structured for learning step-by-step.

Beginners are better off starting with tutorials and guided courses, then returning to Apple's docs later for deeper understanding.

### 3.6 Getting Lost in Objective-C

Objective-C was Apple's primary language before Swift. Unless you're planning to work at Apple or maintain a very old codebase, there's almost no reason to learn it.

The two languages have very little in common. Objective-C has a steep learning curve and lacks many of Swift's modern features. For beginners, it's a waste of time. Focus exclusively on Swift.

### 3.7 Taking Shots at Other Languages

Criticizing JavaScript, Python, Java, or any other language to make Swift seem superior is pointless and counterproductive.

These languages aren't competitors—they each have strengths. In fact, Swift borrows ideas from many of them: Rust's memory safety, Python's syntax clarity, Haskell's functional concepts, React's declarative UI patterns.

Language wars are a distraction. Focus on learning Swift well, and appreciate what other languages bring to the table.

---

## 4. Learning Resources

Here's the important part: you can learn everything you need to become an iOS developer **without spending a dime**.

Expensive courses don't guarantee better results. In fact, platforms like Udemy encourage buying multiple cheap courses through constant sales, leading people to hoard content they never finish. By sticking to free resources, you avoid that trap and can start building real skills without financial risk.

### Structured Tutorials

**Apple's Official Resources:**

1. [**Apple's Teaching Code site**](https://www.apple.com/uk/education/k12/learn-to-code/) - A massive curriculum covering Swift from basics to professional certifications. Takes a bit of exploration to find your starting point, but there's a ton here.
2. [**SwiftUI Tutorials**](https://developer.apple.com/tutorials/develop-in-swift/) - Step-by-step guides for building real apps. You'll need Swift fundamentals first, then dive into these.

**Hacking with Swift (Highly Recommended):**

- **100 Days of SwiftUI** - Teaches Swift fundamentals, then walks you through building 20+ real apps using articles, videos, and interactive tests. Gradual learning curve, comprehensive coverage.
- **100 Days of Swift** - Similar concept but focused on UIKit for those who prefer that path.

### YouTube Channels

Excellent video tutorials for visual learners:

- [**Chris Ching**](https://www.youtube.com/watch?v=VlhcNR7Qrno) - Builds a slots game from scratch
- [**Mark Moeykens**](https://www.youtube.com/watch?v=51xIHDm_BDs) - Explains five essential SwiftUI concepts
- [**Hacking with Swift**](https://www.youtube.com/watch?v=aP-SQXTtWhY&t=1606s) - Teaches Swift and SwiftUI together with live Q&A

### Other High-Quality Sites

- [**Ray Wenderlich**](https://www.raywenderlich.com/)
- [**Donny Wals**](https://www.donnywals.com/)
- [**Antoine van der Lee**](https://www.avanderlee.com/)

Explore different resources to find the teaching style that clicks for you.

### Learning Apps

If you prefer learning on your device:

- **Swift Playgrounds** (Apple) - Interactive lessons on iPad or Mac, from beginner to advanced
- **Unwrap** (Hacking with Swift) - Teaches Swift fundamentals through videos, tests, and practice exercises

### Finding Answers

When you get stuck, these communities are beginner-friendly and eager to help:

- **Hacking with Swift Forums**
- **iOS Dev Happy Hour**
- Slack groups (see community section below)
- Stack Overflow (though it can be less welcoming to beginners)

---

## 5. Connecting to the Community

Connecting with the iOS development community isn't optional—it's essential. This is how you meet peers, learn more effectively, discover job opportunities, and stay motivated.

### Who to Follow on Twitter/X

A simple way to plug into the community is following developers who share their work and highlight others. Here are 10 accounts worth following:

- **Sean Allen** - YouTube creator sharing tutorials and weekly dev highlights
- **Antoine van der Lee** - iOS developer posting GitHub resources and newsletters
- **Novall Khan** - Apple engineer sharing project videos and learning experiences
- **Steve Troughton-Smith** - Highlights impressive iOS projects and his own app development
- **Kaya Thomas** - Indie developer featured by Apple; tweets about work, books, and articles
- **Majid Jabrayilov** - Swift/SwiftUI blogger curating community ideas
- **Donny Wals** - Swift author sharing practical tips and encouraging project sharing
- **Sommer Panage** - Apple accessibility team; tweets insights for building better apps
- **Natascha Fadeeva** - Writes about Core Data, interview prep, and iOS development
- **Paul Hudson** (Hacking with Swift) - Shares projects, tutorials, and community highlights

Following these accounts exposes you to tutorials, projects, discussions, and inspiration as you learn.

### Newsletters, Forums, Meetups, and More

**Newsletter:** [**iOS Dev Weekly**](https://iosdevweekly.com/) - Over 500 issues of tips, tutorials, and news every week

**Forum:** [**Hacking with Swift Forums**](https://www.hackingwithswift.com/forums) - Beginner-friendly with tons of categories for questions and sharing

**Zoom Meetup:** [**iOS Dev Happy Hour**](https://www.iosdevhappyhour.com/) - Monthly calls with 300+ attendees, breakout rooms for personal chats

**Conference:** [**Apple's WWDC**](https://developer.apple.com/wwdc25/) - Major annual event for learning and networking, with many side events listed in [this GitHub repo](https://github.com/twostraws/wwdc)

**Slack Group:** [**Hacking with Swift Slack**](https://hackingwithswift.slack.com/join/shared_invite/zt-3fkgtqzok-3wQokMcNg80XWBt_3pSgaA) - Free channels for Swift, SwiftUI, 100 Days, and more—ideal for quick answers

Engaging in any of these helps you stay informed, meet peers, and get support throughout your learning journey.

---

## 6. How Long It Will Take

The question everyone asks: **How long does it take to go from zero to landing an entry-level iOS job?**

The honest answer is "it depends"—but let's break that down into something actually useful.

### The Golden Rule: Don't Rush

Trying to tackle multiple courses at once or cramming for hours every day rarely works. Learning Swift and building apps requires trial, error, and repeated practice. Mistakes aren't setbacks—they're how you develop deep understanding.

Take your time. Experiment with projects. Explore tangents. Don't be afraid to revisit topics. Depth beats speed every time.

### What's Your Background?

Your timeline depends heavily on what you already know:

**CS Degree:** You already understand variables, loops, arrays, OOP, data structures, and algorithms. This can cut **4-6 months** off your learning path and helps with job applications.

**Coding Bootcamp:** You'll have many fundamentals too, shaving **3-4 months** off your timeline, though some companies may still prefer degrees.

**Self-Taught Experience:** If you've coded in your spare time with other languages or frameworks, that can reduce your learning by **1-2 months**.

**No Prior Experience:** Expect roughly **9-12 months** to go from zero to entry-level iOS developer, especially if you're working another job alongside learning.

With focused, deliberate effort, some learners have landed jobs in under two months. A CS degree isn't required, but consistent hard work absolutely is.

### Cut Yourself Some Slack

It takes as long as it takes. Life happens—stress, family, unexpected events. Don't beat yourself up if you fall behind.

Progress isn't measured by a fixed timeline. Whether you land a job in 50 days, 500 days, or longer, the important thing is that you keep going. Resilience matters more than speed.

---

## 7. Preparing to Apply for a Job

When you're getting close to applying for that first iOS job, there's a huge collection of free resources to help you succeed.

**Hacking with Swift Career Guide:** [https://www.hackingwithswift.com/career-guide](https://www.hackingwithswift.com/career-guide)

This includes:

- Interactive skill reviews to test your knowledge
- Coding tests used in real-world interviews
- Over 200 commonly asked interview questions with suggested answers
- Articles on finding jobs, preparing for interviews, and more

**Sean Allen's Interview Prep:** [YouTube playlist](https://www.youtube.com/playlist?list=PL8seg1JPkqgF5wazzCKSq3EEfqt3t8mvA) covering classes vs structs, functional programming, error handling, and more—short, focused videos to get you interview-ready.

---

## Where Now?

We've covered the core and extension skills, common mistakes, free resources, community connections, and interview prep. That's a lot—hopefully it's given you a clear roadmap.

**The most important takeaway:** You have access to an enormous amount of high-quality, free material. It's tempting to spend hundreds on courses, but you don't need to. Start with free resources, build momentum, and find a teaching style that works for you. Once you're confident and progressing well, then decide if investing money makes sense.

For now:

1. Focus on taking consistent action
2. Build real projects
3. Stay connected with the community
4. Don't rush, but don't stop

Best of luck on your iOS development journey. You've got this.

---

**Next:** [Day 1 - First steps in Swift](Day%201%20-%20First%20steps%20in%20Swift.md)
