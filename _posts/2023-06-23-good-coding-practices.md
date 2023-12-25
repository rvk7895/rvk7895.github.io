---
layout: post
title: Good Coding Practices
date: 2023-06-23 11:12:00-0400 
description: Collection of coding practices that I realize myself or learn from others.
tags: coding
categories: coding
---

# Write better log statements

As a developer, you might assume that adding logging statements is an easy job. After all, what's there to it? Just write a simple `print` or `System.out.println()` statement, and you're done, right? (Although, let's be real, Java, what's up with that unnecessarily long syntax?)

However, the reality is that logging is one of the most essential tools for debugging your code. Without proper logging, you can't easily track down bugs, troubleshoot issues, or understand how your code is behaving in different environments. And if you're not writing descriptive log statements that are easily understood by other developers, then it's as good as a knitted condom - it might look good, but it's not going to be helpful when you really need it.

So, how can you write better log statements? How can you make sure that your logging is effective and helpful for you, your team, and any other developers who might need to work with your code? Here are some tips to consider:

- Use meaningful log messages that provide information about the context in which the log message is being generated. This can include details like the function or method where the log message is being generated, the values of any relevant variables, and any other contextual information that might be helpful for understanding what's happening.
- Use log levels appropriately to indicate the severity of the log message. For example, you might use a "debug" log level for messages that are only relevant for developers, and a "warning" log level for messages that indicate potential issues that should be addressed.
- Avoid using hard-coded strings in log messages. Instead, use constants or variables to make log messages more maintainable and easier to read. This can help ensure that all your log messages are consistent and use the same terminology, which can be especially helpful when you're dealing with a complex codebase.
- Be consistent in your log message format and structure. This can include details like the order of the message components, the use of whitespace and line breaks, and any other formatting details that can help make your log messages more readable and consistent.
- Include relevant information such as the timestamp, thread ID, and relevant metadata. This can help you track down issues and understand how your code is behaving in different environments.
- Use log aggregation tools such as Logstash, Fluentd, or Splunk to centralize and analyze your logs. These tools can help you easily search through your logs, identify patterns or trends, and troubleshoot issues more quickly and effectively.

By following these tips, you can improve your logging and make it more effective for your team and your codebase. So, don't skimp on your logging - take the time to write clear, concise, and helpful log messages that will make your life easier in the long run.

---

# Close your goddamn resources

One of the most common reasons on why builds in production fail is because of improper usage of the resources. A typical application nowadays would deal will deal with a lot of resources like **Files, Sockets and Database Connections**. These resources need to be handled with a lot of care, because an operating system allocates the application system resources, and typically systems have an upper limit on this. If for some reason the program fails or an exception occurs and the resources aren’t freed up, it keeps on building up, and this leads to application servers being frequently restarted when resource exhaustion occurs. 

For any resources that has been opened, a corresponding call to its `close()` method should be made. Also effective use of `try/catch/finally` is needed when dealing with outside resources, to make sure that any execution path leads to proper closing of the resources also. 

Many programming languages like Python and Ruby provide language level facilities which allow automatic handling of resources. In Java also a programming construct known as `try-with-resource` allows an effective way for automatic handling of resources. The syntax for `try-with-resource` is as follows:

```java
try(
			//Resource 1 Declaration;
			// .
			// .
			//Resource n Declaration;
		) {
				// code
			}
catch {
				//code
			}
```