# Refactor Friday! Anansi XMLEngine

Welcome to the first Refactor Friday! In this blog series I'll take on one refactoring task at a time and explain:

*   Why I chose to refactor this area
*   How I plan to improve it

Today I'm refactoring [Anansi](https://github.com/christian-westbrook/anansi), the open-source blogging and portfolio engine!

# Motivation

I launched christianwestbrook.dev so that I could share my ideas about the things I love doing. These things usually involve writing software! 

Anansi is a minimalist blogging platform that's mostly written in PHP. 

# Situation

Anansi is a minimalist blogging platform that's mostly written in PHP. In fact, this site is a deployment of Anansi! It's purpose is simple: to give authors a simple, open-source solution for blog hosting.

To provide the actual blog feed, Anansi reads from a store of XML files representing blog posts. Each blog's file stores content written in Markdown, which is easy to deal with when writing. I love that [my favorite writing tool](https://hemingwayapp.com/) supports exporting text as Markdown content!

Web browsers don't understand Markdown, though. Before we can render a blog post in all its glory, we need to convert it to a format the browser can understand. We need a way to parse Markdown content and generate equivalent HTML content. Cue the XMLEngine class!

XMLEngine is a PHP class that provides services related to Anansi's blog files. Its implementation, however, deals with XML, Markdown, and HTML! It appears to represent the blogs themselves rather than an XML processing tool. This is clearly an overloaded class with room for improvement, but I'll save that for another blog!

# Problem Statement

XMLEngine provides a method that performs Markdown to HTML conversion. This method uses [regular expressions](https://en.wikipedia.org/wiki/Regular_expression) to decide what exactly a given line represents. Is it a heading? An image?

Once the method knows the kind of line it's looking at, it can follow a series of rules for converting that line to HTML. These rules cover several different kinds of lines. Because of this, the method is getting long and becoming difficult to read.

It's hard to overstate how important it is for code to be readable. Humans read source code fare more often than they write it. You will read your code several times for every chance you have to refactor it. One of the easiest ways to up your game as an engineer is to write readable code.

# Design

Anansi currently supports the following Markdown content:

*   Headings
*   Images
*   Hyperlinks
*   Bold and italic text
*   Unordered lists
*   Newlines
*   Plain text

A simple improvement here would be to grab groups of related statements and combine them. I'll create a series of new methods for handling each of these kinds of content. In today's refactor I'll tackle the first two cases: headings and images.

# Implementation

I started by creating a new method for converting Markdown headings to HTML. I replaced the original block of code with a single regular expression check and method call. This task made some potential improvements immediately visible.

SHOW BEFORE AND AFTER

In Markdown we use number signs (#) to represent headings. Their count is important: more number signs should result in a smaller heading. In the original logic I used several regular expression checks to identify headings. Each check mapped to a heading of a certain size.

SHOW REGEX CHECKS BEFORE

Reviewing this code made me realize that it would be a lot simpler to just count number signs! Given the initial check, I can assume that I'm working with a heading once inside the new method. I was able to compress all this logic into four lines by depending on that assumption. The new method counts the number signs used to define the heading and creates a new heading in HTML of the same size.

SHOW NEW SMALLER LOGIC

Next I created a second method for handling images. I replaced the original block of code with a similar check and call to the new method.

SHOW BEFORE AND AFTER

The new conversion logic is a lot like the original. I'm not entirely sure why, but the original handles a case where the image is embedded in the middle of a line. The main improvement here is a new assumption that images will exist on their own lines.

SHOW NEW METHOD

# The Verdict

I really like how these two new methods turned out! You can see the diffs yourself [here](https://github.com/christian-westbrook/anansi/pull/45/files). I felt uneasy about XMLEngine's design, but this tiny refactor sparked new ideas I'm excited to try!

Just within this method other kinds of supported Markdown content needs isolated. Some of them will be trickier to handle. For instance, unordered lists stretch across many lines. That should make for a more interesting refactor.

The XMLEngine class itself should be a candidate for a future refactor. Its abstraction could represent a single blog post and supporting operations. I could even split it into multiple classes if it would be useful to isolate common support methods.

Looking back at your code is an opportunity to learn. Are your solutions easy to understand when they're revisited? Can you derive why you made key design decisions? Are there any that you would change today? Take advantage of seeing your work from the maintainer's point of view, and then make it easy to maintain.

**Thanks for joining me** on my first Refactor Friday! I'm looking forward to using this format again to share examples of how I think about refactoring.

How did I do? Would you have done something differently? I'd love to hear your thoughts! As always, feel free to reach out to me at [christianwestbrook@live.com](mailto:christianwestbrook@live.com)

Signing off,

Christian