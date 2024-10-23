---
title: Cognitive Walkthrough of a Vending Machine
layout: doc
---

# Cognitive Walkthrough of a Vending Machine

In lecture 13, we learned about different methods of evaluating design. One of these methods was the cognitive walkthrough. While it may not be as effective at exposing design flaws as a user study, the guiding questions can nevertheless be useful when thinking about what a user's experience will be like. In particular, I found that examining the ATM closely in class to be a helpful exercise. Listening to everybody's input about what they expected to happen and how they expected to interact with the machine made it clear that there were improvements that could have been made.

For this blog post, I've decided to do a cognitive walkthrough of a vending machine, which I selected because it is something that I interact with often enough to have a good idea of how to use it, but not enough to have built muscle memory (which might have made it harder to conduct this exercise with an app that I use multiple times per day, for example). To avoid broadening the scope of this post too much, I will only be evaluating one task, which is simply to purchase an item from the vending machine. This can be accomplished with the following sequence of actions:

1. Provide payment
2. Select an item to purchase

It's worth nothing that on many vending machines, the order of these items can also be swapped. Now, let's answer the guiding questions for each step.

## Provide payment

![The initial screen on the vending machine](/../assets/images/vending1.jpg)

1. Based on what we see when we approach the machine, it's clear that something needs to be done, but it might not be obvious what that is. This is because the screen is saying that we should begin by touching it, but the card reader is saying we should begin by swiping/tapping our card. I decided to tap my card.
2. I would say that touching the screen and tapping a card are both correct actions. In the case of the screen, it is obvious that the action is available since there is a message, and it is clear that touching the screen will start the process. In the case of tapping your card, it is less obvious, since the text is smaller on the card reader, and it stays the same stagnant green color throughout the process. The user is less likely to pay attention to it.
3. After the user taps their card, the user is told that their balance is $5.00 and the screen changes to a keypad where the user can input the code for the item they want, which makes it clear that this is the right thing to have done.
4. Since selecting an item is the next logical step in the process, the user would know that they have made progress towards their goal.

## Select an item to purchase

![The item selection screen](/../assets/images/vending2.jpg)

1. The user can easily see that the next step is to enter a code, since they are presented with a keypad and three blank spaces.
2. The user is prompted to enter a three digit code. Since they can see that each item in the display is labeled with a three digit code, and because this is also how most vending machines work, the user will likely know that to select an item, they have to enter the corresponding code. However, it could be clearer to include a simple message like "Select an item," since there is no text that explicitly tells the user what to do.
3. Once they enter a three digit code, the user is taken to a screen where they are asked to pay. Although being taken to a different screen without an alert would normally imply that they have done the last step correctly, in this case the user may think that they have done something wrong in the process, since they already provided their payment in the beginning.
4. It is not obvious that progress has been made towards the larger goal, because it is at this point that the user probably would have expected to be finished with the process. Since the user has already paid, they may wonder if they have to repeat the process again.

![The payment screen](/../assets/images/vending3.jpg)

However, once the user again provides their payment, the item that they selected is dispensed. If I had instead touched the screen in the beginning, it might have been a more straightforward process in which I would only have to provide my payment once. This cognitive walkthrough has provided useful insights that would be helpful for someone designing this vending machine:
* If the user is meant to start by touching the screen, it would be best to avoid having a message on the card reader that says otherwise.
* If the user decides to start by providing payment, the payment information can be temporarily stored somewhere until the transaction is completed or canceled so that the user does not have to provide payment twice.
* The screen that asks the user to input a code could include a message to clarify what the user is meant to do.