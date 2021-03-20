## Difference between merge and rebase

Let's say we have a master with the following commits `1->2->3` and<br>
feature branch with the following commits `1->4->5`. If we do `git merge feature`<br>
then git creates a separate commit that contains a content of commits **4** and **5** from feature and name it `45`. <br>
So now master looks like this `1->2->3>45`
