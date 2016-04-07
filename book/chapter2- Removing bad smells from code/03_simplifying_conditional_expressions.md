
Conditional logic has a way of getting tricky, so here are a number of refactorings you can use to
simplify it. The core refactoring here is Decompose Conditional, which entails breaking a
conditional into pieces. It is important because it separates the switching logic from the details of
what happens.

###Decompose Conditional

**Name**: Decompose Conditional

**Summary**
You have a complicated conditional (if-then-else) statement. You will apply this refactoring to simplify your code readability.

**Motivation**
- One of the most common areas of complexity in a program lies in complex conditional logic. 
- As you write code to test conditions and to do various things depending on various conditions, you
quickly end up with a pretty long method. Length of a method is in itself a factor that makes it
harder to read, but conditions increase the difficulty.
- The problem usually lies in the fact that the code, both in the condition checks and in the actions, tells you what happens but can easily obscure why it happens.
- As with any large block of code, you can make your intention clearer by decomposing it and
replacing chunks of code with a method call named after the intention of that block of code. With
conditions you can receive further benefit by doing this for the conditional part and each of the
alternatives. This way you highlight the condition and make it clearly what you are branching on.
You also highlight the reason for the branching.

**Mechanics**

- Extract the condition into its own method.
- Extract the then part and the else part into their own methods

**Example**

Suppose I'm calculating the charge for something that has separate rates for winter and summer:

```
if (date.before (SUMMER_START) || date.after(SUMMER_END))
   charge = quantity * _winterRate + _winterServiceCharge;
else charge = quantity * _summerRate;
```
I extract the conditional and each leg as follows:

```
if (notSummer(date))
   charge = winterCharge(quantity);
else charge = summerCharge (quantity);

private boolean notSummer(Date date) {
   return date.before (SUMMER_START) || date.after(SUMMER_END);
}
private double summerCharge(int quantity) {
   return quantity * _summerRate;
}
private double winterCharge(int quantity) {
  return quantity * _winterRate + _winterServiceCharge;
}
```

###Consolidate Conditional Expression

**Name**
Consolidate Conditional Expression

**Summary**
You have a sequence of conditional tests with the same result, you will apply this refactoring to simplify your code.

**Motivation**

**Why**
- Sometimes you see a series of conditional checks in which each check is different yet the
resulting action is the same. When you see this, you should use ands and ors to consolidate them
into a single conditional check with a single result
-Consolidating the conditional code is important for two reasons. First, it makes the check clearer
by showing that you are really making a single check that's oring the other checks together. The
sequence has the same effect, but it communicates carrying out a sequence of separate checks
that just happen to be done together. The second reason for this refactoring is that it often sets
you up for Extract Method. Extracting a condition is one of the most useful things you can do to
clarify your code. It replaces a statement of what you are doing with why you are doing it.

**Why NOT**
- The reasons in favor of consolidating conditionals also point to reasons for not doing it. If you
think the checks are really independent and shouldn't be thought of as a single check, don't do
the refactoring. Your code already communicates your intention.

**Mechanics**

- Check that none of the conditionals has side effects.If there are side effects, you won't be able to do this refactoring.
- Replace the string of conditionals with a single conditional statement using logical
operators.
- Compile and test.
- Consider using Extract Method on the condition.

**Example**

```
double disabilityAmount() {
  if (_seniority < 2) return 0;
  if (_monthsDisabled > 12) return 0;
  if (_isPartTime) return 0;
  // compute the disability amount & return
```

Here we see a sequence of conditional checks that all result in the same thing. With sequential code like this, the checks are the equivalent of an or statement:

```
double disabilityAmount() {
    if ((_seniority < 2) || (_monthsDisabled > 12) || (_isPartTime))
       return 0;
    // compute the disability amount & return
```

Now I can look at the condition and use Extract Method to communicate what the condition is
looking for:

```
double disabilityAmount() {
  if (isEligibleForDisability()) return 0;
  // compute the disability amount
...
}

boolean isEligibleForDisability() {
   return ((_seniority < 2) || (_monthsDisabled > 12) || (_isPartTime));
}
```

### Anding Conditional Expressions

Above example showed ors, but I can do the same with ands. Here the set up is something like the
following:

```
if (onVacation())
   if (lengthOfService() > 10)
     return 1;
return 0.5;
```
This would be changed to

```
if (onVacation() && lengthOfService() > 10) 
   return 1;
else return 0.5;
```

You may well find you get a combination of these that yields an expression with ands, ors, and
nots. In these cases the conditions may be messy, so I try to use Extract Method on parts of the
expression to make it simpler.

If the routine I'm looking at tests only the condition and returns a value, I can turn the routine into a single return statement using the ternary operator. So

```
if (onVacation() && lengthOfService() > 10)
   return 1;
else return 0.5;
```
becomes

```
return (onVacation() && lengthOfService() > 10) ? 1 : 0.5;
```
