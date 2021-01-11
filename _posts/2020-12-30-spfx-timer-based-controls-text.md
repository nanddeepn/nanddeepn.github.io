---
title: "Timer based Button text in SharePoint Framework"
date: "2020-12-30"
share: true
header:
  image: media/2020-12-30-spfx-timer-based-controls-text/01.gif
  teaser: media/2020-12-30-spfx-timer-based-controls-text/01.gif
categories:
  - SharePoint
  - SharePoint Framework
tags:
  - "2020"
  - December 2020
last_modified_at: 2020-12-30T00:00:00-00:00
---

## Overview

Buttons are the clickable HTML elements to which we can put a static text throughout its life in an application. In some cases, we change the button text based on the state of the application.

In this article, we will explore how we can set a button text based on a ticking timer. Not just for button text, but you can use the same pattern for setting text to any UI controls. We will use `setInterval()` and `clearInterval()` methods.

## Why timer based text for controls?

Well, there are no regular business cases for this as such. However, in some scenarios this can be helpful including below:

1. You have a modal dialog open with some informative text which can take 5 seconds for a normal user to read. You think, your users should spend time reading that information before they simply close the dialog.
2. Hurry up your participants to submit their response in a Quiz based application.

## Show me the code

Let us get into the code part by scaffolding React based SPFx solution.

**React state**

Define the state as below:

```typescript
export interface ITimeControlsState {
    counter: number;
    buttonText: string;
}
```

- `counter` represents the incremented timer value to be shown as control text.
- `buttonText` represents the text to be set to button (or any control).

In the constructor, set the default values:

```typescript
constructor(props: ITimeControlsProps) {
    super(props);

    this.state = {
      counter: 1,
      buttonText: "1"
    };
}
```

**Implement Code**

At class level, declare a variable `intervalId` to hold the ID value of the timer that is set. Later, we will use this ID value to cancel the timer.

```typescript
private intervalId: number;
```

In the `componentDidMount()` method, start by setting the interval.

```typescript
public componentDidMount() {
    const thisBoundedIncrementer = this.incrementCounter.bind(this);
    this.intervalId = setInterval(thisBoundedIncrementer, 1000);
}

private incrementCounter(): void {
    if (this.state.counter <= 5) {
      const { counter } = this.state;
      this.setState({ counter: counter + 1, buttonText: (counter + 1).toString() });
    }
    else {
      this.setState({ buttonText: "Close" });
      clearInterval(this.intervalId);
    }
}
```

Clear the interval, when the timer value is reached.

## The Outcome
Here is how the final implementation looks like.

![](/media/2020-12-30-spfx-timer-based-controls-text/01.gif)


## Summary
We can set a button text based on a ticking timer using `setInterval()` and `clearInterval()` methods.

## Code download

The code developed during this article can be found here:

[https://github.com/nanddeepn/code-samples/tree/master/SPFx/WebParts/spfx-timer-based-controls-text](https://github.com/nanddeepn/code-samples/tree/master/SPFx/WebParts/spfx-timer-based-controls-text)


## References
- [setInterval() Method](https://www.w3schools.com/jsref/met_win_setinterval.asp)
- [clearInterval() Method](https://www.w3schools.com/jsref/met_win_clearinterval.asp)
- [Use setInterval Callback with Context in React](https://app.pluralsight.com/guides/how-to-use-setinterval-callback-with-context)
