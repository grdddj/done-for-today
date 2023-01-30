# done-for-today

Implementing an idea from an excellent book [Deep Work](https://www.goodreads.com/book/show/25744928-deep-work) by Cal Newport.

The idea can be paraphrased like this:
- do the most amount of really focused (deep) work during the working hours, with as little distractions as possible
- disconnect completely from work during the rest of the day to recharge focus and energy

To create a strong boundary between work and rest, creating a ceremony of evaluating the day and planning the next day. This should transfer all the unfinished tasks to the next day, together with concrete steps of how to achieve them.

Ending the working day has never been easier than typing `iamdone` and `donefortoday`!

```
alias iamdone='code ~/done-for-today'
... modify the plan.md file ...
alias donefortoday='git add . && git commit -m "Done for `date +%m/%d/%Y` at `date +%H:%M`" && git push'`)
```
