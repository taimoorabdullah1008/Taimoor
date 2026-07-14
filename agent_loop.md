# The Agent Loop

An AI agent doesn't just make one guess and stop. It works in a loop,
constantly checking whether its actions are working and adjusting if they
aren't. This loop has five steps:

1. **Observe** — Look at the current situation and gather relevant data.
2. **Decide** — Pick the best action based on what was observed.
3. **Act** — Carry out that action.
4. **Get Feedback** — Check what happened as a result of the action.
5. **Improve** — Use that feedback to make a better decision next time.

Then the loop repeats, starting again from Observe.

```
 ┌─────────┐     ┌────────┐     ┌─────┐     ┌──────────────┐     ┌─────────┐
 │ Observe │ --> │ Decide │ --> │ Act │ --> │ Get Feedback │ --> │ Improve │
 └─────────┘     └────────┘     └─────┘     └──────────────┘     └─────────┘
      ^                                                                │
      └────────────────────────────────────────────────────────────────┘
```

## Example: Helping Ayesha finish her assignments

Let's walk through the loop using a student named Ayesha.

### Round 1

**Observe**
The agent looks at Ayesha's activity data:
- She hasn't logged in for 9 days.
- She has completed only 1 out of 4 assignments.

**Decide**
Based on this, the agent decides that a gentle nudge is the right first move.
It recommends: **"Send Ayesha a reminder email."**

**Act**
The agent sends the reminder email.

**Get Feedback**
The agent checks back later: Ayesha still hasn't logged in, and she didn't
respond to the email.

**Improve**
The email alone didn't work, so the agent learns that this approach isn't
enough for Ayesha. It updates its plan for the next round: instead of just
repeating the same email, it should try a stronger or different action, such
as:
- Sending a text message or push notification instead of email.
- Notifying her teacher or mentor so a real person can check in.
- Offering a smaller, easier task first to help her re-engage gradually.

### Round 2

**Observe**
The agent observes that Ayesha still hasn't logged in, and the email had no
effect.

**Decide**
This time, it decides on a new recommendation: **"Notify Ayesha's teacher to
follow up personally, and suggest she start with just one small
assignment."**

**Act**
The agent notifies the teacher and sends Ayesha a simplified task suggestion.

**Get Feedback**
The agent waits to see whether this new approach leads to Ayesha logging in
or completing work.

**Improve**
Whatever happens next, the agent keeps learning: if this works, it remembers
that a personal check-in was more effective than an email for Ayesha. If it
still doesn't work, it tries yet another approach next time.

## Why this matters

The key idea is that the agent doesn't just make one recommendation and give
up. It keeps observing results, decides on the next best step, and improves
its strategy each time based on real feedback — getting smarter about what
actually works for each person.
