### 15. Deprecation

- Spending time on removing something can make sense because otherwise you will always need to update it when connected systems change
- > There’s an old joke within Google that there are two ways of doing things: the one that’s deprecated, and the one that’s not-yet-ready
- Designing a system in a way that it can be deprecated might feel weird but it is common in other engineering disciplines. Think of a nuclear power plant
- Questions to ask yourself when designing
	- How easy will it be for customers to migrate to a potential replacement?
	- Are there ways to incrementally replace parts of the system?
- When something becomes deprecated, you can print warnings for customers that still use it. However, often these just pile up and are mostly ignored. This is also known as [alarm fatique](https://en.wikipedia.org/wiki/Alarm_fatigue)
- Good warning messages are *actionable* (i.e. you can tell what to do) and *relevant*
- You should automatically alert people when they are using deprecated stuff in new code (*backsliding prevention*). You could e.g. surface this warning via the code review tool and automatically offer fixes
- There should be incremental milestones for deprecating a system, just like there's milestones for building a new system
