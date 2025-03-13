

![image-20230403132318319](attachments/cs161.assets/image-20230403132318319.png)

---

## 4/3 L1: Multiplication

* Grade school multiplication is when you multiply each digit of the second by all digits of the first number, shifting to the left for each digit.

![image-20230403142357788](attachments/cs161.assets/image-20230403142357788.png)

* Grade-school multiplication algorithms have about $n^2$ one-digit operations.
  * Every digit of the bottom number has to multiplied by every digit of the top.
* When something "scales like $n^2$", that means it is roughly a quadratic.

![image-20230403142905380](attachments/cs161.assets/image-20230403142905380.png)

* As $n$ gets bigger, an algorithm that runs in time $O(n^{1.6})$ is better than an $O(n^2)$.

### Divide and Conquer

![image-20230403143048760](attachments/cs161.assets/image-20230403143048760.png)

* One approach of divide and conquer is to break up the multiplication into smaller chunks of smaller digit multiplications.

![image-20230403143101196](attachments/cs161.assets/image-20230403143101196.png)

* Suppose that $n$ is even, we have the following general algorithm:

![image-20230403143232890](attachments/cs161.assets/image-20230403143232890.png)

* Notice that this 4-digit multiplication problem breaks into four 2-digit multiplication problems.

  * Each of those 2-digit multiplication problems breaks up into another four 1-digit multiplications. This is $4^2 = 16$ one-digit multiples.

  ![image-20230403143443300](attachments/cs161.assets/image-20230403143443300.png)

![image-20230403143628251](attachments/cs161.assets/image-20230403143628251.png)

* Notice that this implementation is worse than the traditional grade school algorithm.
* For 8 digits, it would be 64 one-digit multiplications.

![image-20230403144135726](attachments/cs161.assets/image-20230403144135726.png)

* We'll end up doing $n^2$ one-digit multiplications, which implies that the running time of this algorithm is AT LEAST $n^2$ operations.

![image-20230403144218623](attachments/cs161.assets/image-20230403144218623.png)

### Karatsuba

![image-20230403144410335](attachments/cs161.assets/image-20230403144410335.png)

* We need $ac$, $ad + bc$, and $bd$.

![image-20230403144424227](attachments/cs161.assets/image-20230403144424227.png)

* Now, we only need to compute three things: $ac$, $bd$, and $(a + b)(c+d)$. This is because we can subtract off the first two from the third to get our desired terms.
* Now assume $n$ is a power of two.

![image-20230403144535730](attachments/cs161.assets/image-20230403144535730.png)

* When analyzing runtime, if we were very rigorous we would also count the number of additions and subtractions. In this case though, we only count the number of one-digit multiplications.

![image-20230403144729023](attachments/cs161.assets/image-20230403144729023.png)

* We now have a branching factor of $3$ instead of $4$ like before.

![image-20230403144808955](attachments/cs161.assets/image-20230403144808955.png)

---

## 4/5 L2: Asymptotic Notation, MergeSort

* Today we focus on more rigorous analysis of algorithms.

### Sorting

* For our considerations, the length of a list is $n$. Sorting algorithms just sort them into an order.

![image-20230406200823346](attachments/cs161.assets/image-20230406200823346.png)

![image-20230406200845326](attachments/cs161.assets/image-20230406200845326.png)

### Worst-case Analysis

* This answers the question of "Does it work?"

![image-20230406200950436](attachments/cs161.assets/image-20230406200950436.png)

* In real life, seeing it work on most inputs is enough. Worse-cast analysis is a stronger proof of that.

![image-20230406201139671](attachments/cs161.assets/image-20230406201139671.png)

#### Proof

* This is how we would rigorously prove that insertion sort works.

![image-20230406201333951](attachments/cs161.assets/image-20230406201333951.png)

* The above is like one step of our logic. We can just repeat that step for our proof.

![image-20230406201417586](attachments/cs161.assets/image-20230406201417586.png)

* This is a case of ==Proof by Induction==

![image-20230406201641126](attachments/cs161.assets/image-20230406201641126.png)

* In the picture above, at the end of the second iteration, the first three elements are in sorted order.

#### Summary

![image-20230406202015589](attachments/cs161.assets/image-20230406202015589.png)

### Asymptotic Analysis

* This answers the question of "Is it fast?"

#### Simple Answer to the Question

![image-20230406203244249](attachments/cs161.assets/image-20230406203244249.png)

![image-20230406203305675](attachments/cs161.assets/image-20230406203305675.png)

* Note that in the above, it was the same algorithm but implemented differently. We saw differences in running time in the above.
  * This doesn't tell us much about how "fast" the algorithm actually is, independent of the implementation or the hardware/software.

#### Pedantic Answer

* We count up all the operations of each thing in the program.

![image-20230406203647232](attachments/cs161.assets/image-20230406203647232.png)

* However, this is very tedious, and it might also be slightly different for different implementations.
* We would also need to know how long a bunch of different operations take and add it all up. It's not very helpful.

### Big-O: $O(\dots)$

* This focuses on how the runtime scales with $n$ (the input size), instead of counting the operations outright.
* Note that this is still worst-case analysis. For an arbitrary function, if it runs in $O(n)$ for some inputs and $O(n^2)$ for others, that program is $O(n^2)$.

![image-20230406203446668](attachments/cs161.assets/image-20230406203446668.png)

![image-20230406203837003](attachments/cs161.assets/image-20230406203837003.png)

#### Pros and Cons

![image-20230406203858442](attachments/cs161.assets/image-20230406203858442.png)

#### Informal Definition

![image-20230406203947440](attachments/cs161.assets/image-20230406203947440.png)

![image-20230406204010917](attachments/cs161.assets/image-20230406204010917.png)

#### Formal Definition

![image-20230406204051924](attachments/cs161.assets/image-20230406204051924.png)

* $T(n)$ is $O(g(n))$ if and only if there exists a $c$ and a $n_0 > 0$ such that for every $n \geq n_0$, we have that $T(n) \leq c * g(n)$.
  * The $c * g(n)$ is the constant multiple idea from the definition
  * $c$ and $n_0$ are both in $\R$. In most cases, $c$ will be an integer.

![image-20230406204531534](attachments/cs161.assets/image-20230406204531534.png)

#### Example

![image-20230406204930866](attachments/cs161.assets/image-20230406204930866.png)

* You can also make $T(n)$ and $g(n)$ the same.

![image-20230406205256015](attachments/cs161.assets/image-20230406205256015.png)

#### Bound

* Big-O $O(\dots)$ is an upper bound.
* Give the above definitions, $T(n) = O(n^2)$ and also $T(n) = O(n^3)$. Its runtime can be anything bigger than $O(n^2)$.
* Consider a function that is $O(n)$. That function is also $O(n^2)$.

![image-20230406204644006](attachments/cs161.assets/image-20230406204644006.png)

### Big-Omega: $\Omega (\dots)$

![image-20230406205744177](attachments/cs161.assets/image-20230406205744177.png)

![image-20230406205756214](attachments/cs161.assets/image-20230406205756214.png)

### Big-Theta: $\Theta(\dots)$

![image-20230406205912174](attachments/cs161.assets/image-20230406205912174.png)

### Not Example

* How do we some if something is not $O(n/n^2/n^3/\dots)$

![image-20230406210149839](attachments/cs161.assets/image-20230406210149839.png)

### Proof Summary

![image-20230406210229044](attachments/cs161.assets/image-20230406210229044.png)

### Finding Big-O from Code

![image-20230406210436412](attachments/cs161.assets/image-20230406210436412.png)

### MergeSort

* Remember that Insertion Sort sorts an array in time $O(n^2)$.
* MergeSort uses divide-and-conquer to perform sorting quicker.

![image-20230406210536087](attachments/cs161.assets/image-20230406210536087.png)

![image-20230406210549303](attachments/cs161.assets/image-20230406210549303.png)

* We get an array and break it into two halves. Then we keep breaking it up into halves (recursion) until the array only has one element, and then merge it together and build back up.

![image-20230406210554714](attachments/cs161.assets/image-20230406210554714.png)

![image-20230406210720202](attachments/cs161.assets/image-20230406210720202.png)

#### Proof Outline

![image-20230406210736756](attachments/cs161.assets/image-20230406210736756.png)

#### Runtime

* MergeSort runs in $O(n \mathtt{log}(n))$ time. Remember that $\mathtt{log}(n)$ grows very slowly.

![image-20230406210928939](attachments/cs161.assets/image-20230406210928939.png)

#### Proof

![image-20230406210954028](attachments/cs161.assets/image-20230406210954028.png)

* We look at a random subproblem and we prove its runtime.

![image-20230406211044630](attachments/cs161.assets/image-20230406211044630.png)

![image-20230406211059697](attachments/cs161.assets/image-20230406211059697.png)

![image-20230406211108492](attachments/cs161.assets/image-20230406211108492.png)

* Each node with size $k$ has time $O(k)$. Note that there may be more than $k$ operations done because of all the comparison in the merging algorithm, but asymptotic analysis brings it to $O(k)$.

![image-20230406211437583](attachments/cs161.assets/image-20230406211437583.png)

* Eg: at level 2, each of the nodes takes time $O(n/4)$. Since there are four nodes, this is $O(n)$ for the entire level.

![image-20230406211340028](attachments/cs161.assets/image-20230406211340028.png)

### Recap

![image-20230406211540744](attachments/cs161.assets/image-20230406211540744.png)

---

## 4/10 L3: Recurrences, Master Theorem

### Recap from Last Time

* What does it mean to work and be fast?
  * You find this using worse-case analysis and big-o notation
* How do you analyze correctness of iterative and recursive algs?
  * You do this by using induction!
* How do you analyze the running time of recursive algorithms
  * By writing out a tree and adding up all of the work done.

### Recurrence Relations

* How do we calculate the runtime of a recursive algorithm?

#### Merge Sort, Revisited

![image-20230410153027278](attachments/cs161.assets/image-20230410153027278.png)

* That equation parallels the pseudocode. The $O(n)$ is for the MERGE operation, and the $2 * T\left(\frac{n}{2}\right)$ is for the two recursive calls of the subproblems.
  * Need to solve two problems of size n/2, then also merge it.
* It can make it more difficult mathematically if we keep the $O(n)$ in the formula. As such, we can just replace it with an concrete constant multiple.

![image-20230410153222229](attachments/cs161.assets/image-20230410153222229.png)

* Note that the use of $\leq$ instead of $=$ is also a recurrence relation. It doesn't matter for a conclusion like $T(n) = O(n\log(n))$.

![image-20230410153343105](attachments/cs161.assets/image-20230410153343105.png)

* For example, find $T(n) = O(n\log(n))$.

#### Base Cases

* Recurrence relations need a base case.

![image-20230410153526843](attachments/cs161.assets/image-20230410153526843.png)

* T(1) is O(1) because its a constant.

#### Finding Closed Form

![image-20230410153617375](attachments/cs161.assets/image-20230410153617375.png)

* We want to find a closed form expression for the relations above.
* One way is with the recurrence tree.

![image-20230410153656721](attachments/cs161.assets/image-20230410153656721.png)

##### Exercise 1

![image-20230410154125879](attachments/cs161.assets/image-20230410154125879.png)

* The recursion tree is just like a straight line, where each level one has one subproblem.

![image-20230410154315646](attachments/cs161.assets/image-20230410154315646.png)

##### Exercise 2

![image-20230410154517922](attachments/cs161.assets/image-20230410154517922.png)

* Note that the contribution at each layer is that because the recurrence relation has the $+n$. As such, this means each subproblem does $n$ work, where $n$ is the size of the subproblem.

##### More Examples

![image-20230410155042465](attachments/cs161.assets/image-20230410155042465.png)

### The Master Theorem

* A theorem so we don't have to calculate the runtime from scratch.
* Describes the pattern of the recurrence relations above.
* It's a formula for many recurrence relations (not all)
* The proof is like a generalized tree method

![image-20230410155308149](attachments/cs161.assets/image-20230410155308149.png)

* The $n^d$ is like the amount of work at each problem.
  * The master theorem only applies if the work at each subproblem is polynomial. It won't work if it's like $O(\log(n))$ for example.

#### Technicality

* Notice that the master theorem relies a lot on integer division. If it's not integer division, it can lead to odd things happening.

![image-20230410190703545](attachments/cs161.assets/image-20230410190703545.png)

* The first recurrence relation above is technically what merge sort would be. However, we can ignore the floors and ceilings in this case.

#### Examples

![image-20230410190901228](attachments/cs161.assets/image-20230410190901228.png)

#### Proof

![image-20230410191005816](attachments/cs161.assets/image-20230410191005816.png)

* The base case can be any case, as long as it's some $O(1)$ case. For example, merge sort had the case where the bottom is where the lists have size 1, which was $O(1)$.

* For this proof, suppose that $T(1) = c$ for the recurrence relation.

![image-20230410191538010](attachments/cs161.assets/image-20230410191538010.png)

* Notice that $a$ is the number of subproblems for each level, and its exponential.
* Think of $c \cdot n^d$ as the amount of work per problem, where $n$ is the size of that problem.
  * For level 1, there are $a$ problems, each of size n\b. As such, the amount of work is $ac$(n/b)$^d$.
* We now all up all of the work that was done at each level.

![image-20230410191904018](attachments/cs161.assets/image-20230410191904018.png)

* The formula above was derived from the general case, at the t-th level.

![image-20230410191941305](attachments/cs161.assets/image-20230410191941305.png)

##### Case 1: $a = b^d$

![image-20230410192105479](attachments/cs161.assets/image-20230410192105479.png)

* Note that the $c$, and $\log(b)$ all disappear because they are just constants. We only keep the things that depend on $n$.
* The $+1$ disappears because, although it gives another term on the order of $n^d$, it is ultimately lower order than $n^d \log(n)$ so it gets dropped.
* The final expression is $T(n) \leq \Theta(n^d \log(n))$.

#### Geometric Sums

![image-20230413123327759](attachments/cs161.assets/image-20230413123327759.png)

#### Case 2: $a < b^d$

![image-20230413123549051](attachments/cs161.assets/image-20230413123549051.png)

* Because $\frac{a}{b^d}$ is less than 1, the first term dominates, the sum just becomes some constant.

#### Case 3: $a > b^d$

![image-20230413123700883](attachments/cs161.assets/image-20230413123700883.png)

* In this case, the last term of the geometric sum dominates. Therefore, we can replace the big sum with the last term, and the theta is there to show that there's some additional constants.

![image-20230413124041598](attachments/cs161.assets/image-20230413124041598.png)

### Understanding the Master Theore

![image-20230413124117074](attachments/cs161.assets/image-20230413124117074.png)

![image-20230413124132206](attachments/cs161.assets/image-20230413124132206.png)

* There are two types of intuitions. The three cases correspond to either intuition winning, or the case of a tie between the two.

![image-20230413124224098](attachments/cs161.assets/image-20230413124224098.png)

![image-20230413124249180](attachments/cs161.assets/image-20230413124249180.png)

![image-20230413124304193](attachments/cs161.assets/image-20230413124304193.png)

![image-20230413124320707](attachments/cs161.assets/image-20230413124320707.png)

### The Substitution Method

* Different way to solve recurrence relations, more general than Master Method

![image-20230413124343995](attachments/cs161.assets/image-20230413124343995.png)

#### Example 1 (nlogn)

![image-20230413124436038](attachments/cs161.assets/image-20230413124436038.png)

![image-20230413124447022](attachments/cs161.assets/image-20230413124447022.png)

* We can guess by continuously expanding the recurrence relation until a pattern shows.
* We use $j = \log(n)$ because that's how many times the recurrence relation has to keep going until we reach $T(1)$.

![image-20230413124640641](attachments/cs161.assets/image-20230413124640641.png)

* For the base case, we know that $T(1) = 1$. We just need to check that the expression that we found matches this.
* The above is doing strong induction. It's assuming for all $1 \leq n < k$.

![image-20230413124717357](attachments/cs161.assets/image-20230413124717357.png)

* This step is what they want to see on the homework. Just the theorem and then the proof.

#### Example 2

* This problem is the same as above, but now with a constant in front of the amount of work.

![image-20230413125055933](attachments/cs161.assets/image-20230413125055933.png)

![image-20230413125109541](attachments/cs161.assets/image-20230413125109541.png)

![image-20230413125155518](attachments/cs161.assets/image-20230413125155518.png)

![image-20230413125223142](attachments/cs161.assets/image-20230413125223142.png)

![image-20230413125229423](attachments/cs161.assets/image-20230413125229423.png)

![image-20230413125300838](attachments/cs161.assets/image-20230413125300838.png)

![image-20230413125306289](attachments/cs161.assets/image-20230413125306289.png)

### Summary

* We have two methods because sometimes the Substitution method works when the master method does not work.

---

## 4/12 L4: Median and Selection

### Summary of Last Lecture

#### Recurrence Relations

![image-20230415121228566](attachments/cs161.assets/image-20230415121228566.png)

#### Master Theorem

![image-20230415121247340](attachments/cs161.assets/image-20230415121247340.png)

#### Substitution Method

![image-20230415121318850](attachments/cs161.assets/image-20230415121318850.png)

### More Substitution Method

![image-20230415121532718](attachments/cs161.assets/image-20230415121532718.png)

* The master theorem does not hold because the subproblems are not the same size.

#### Step 1

![image-20230415121628094](attachments/cs161.assets/image-20230415121628094.png)

* If we try to keep solving the recurrence and substituting back in, it can get kind of ugly.
* We use the iPython notebook to just guess it.

#### Step 2

* We now try to move it. However, note the following

![image-20230415121916926](attachments/cs161.assets/image-20230415121916926.png)

* In the above, the inductive hypothesis says it holds for that particular $n$. However, the definition states that it holds for all sufficiently large $n$.
* Instead, we should pick $C$ and $n_0$ first.

![image-20230415122210086](attachments/cs161.assets/image-20230415122210086.png)

* Do the whole proof, leaving $C$ as undetermined. Then, work backwards to find $C$.
* The base case tells us that $C \geq 1$.
* In the inductive step, we do strong induction and assume it holds for all $n$ so that $1 \leq n < k$. We want to show that the IH also holds for $k$.
  * We apply our inductive hypothesis to the $T(k/5)$ and $T(7k/10)$ recurrences.
  * Now our recurrence relation is $T(k) \leq k + C/5 \cdot k + 7C/10 \cdot k$. We want to show that this is less than or equal to $Ck$ for our inductive hypothesis.

![image-20230415122555327](attachments/cs161.assets/image-20230415122555327.png)

* Therefore, if $c$ is at least 10, then our formula holds. As such, pick $c = 10$.
* If we make the wrong guess, it's likely that there is no $c$ that is a solution for this.

#### Step 3

![image-20230415232441704](attachments/cs161.assets/image-20230415232441704.png)

#### Summary and Aside

* If we want to show that $f(n) = O(g(n))$, can we look at limit n to infinity $f(n) / g(n)$?
  * Yes, we can.
* We use the other methods for recurrence relation because we don't have a closed form statement for $f(n)$.

![image-20230415234014897](attachments/cs161.assets/image-20230415234014897.png)

### k-SELECT Problem

* You have an array $A$, and a $k$. This problem returns the kth smallest element of $A$.

![image-20230415234134887](attachments/cs161.assets/image-20230415234134887.png)

#### O(nlog(n))

* We can solve the problem in nlogn time. To do so, just simply sort it using merge sort and then get the kth element.

![image-20230415234240014](attachments/cs161.assets/image-20230415234240014.png)

#### O(n)

* We can't do better than O(n) because we have to at least visit element in the array in order to find the kth smallest.

![image-20230415234329291](attachments/cs161.assets/image-20230415234329291.png)

* In the code above, we use a variable to keep track of the minimum value.

![image-20230415234356456](attachments/cs161.assets/image-20230415234356456.png)

* In the above, we use a variable to keep track of the minimum value and the second smallest value.
* Another way to do it is to run `SELECT(A, 1)`, then remove that element, then run `SELECT(A, 1)` again.

![image-20230415234511100](attachments/cs161.assets/image-20230415234511100.png)

* It gets much harder if we decide to generalize this problem.

### k-SELECT Solution

* Let's try divide and conquer!
* We do this much like quick sort.

![image-20230415235345248](attachments/cs161.assets/image-20230415235345248.png)

* We partition the array into two smaller arrays, moving around the pivot.

![image-20230415235420113](attachments/cs161.assets/image-20230415235420113.png)

* Now consider our choices of $k$ given each of these arrays.

![image-20230415235602744](attachments/cs161.assets/image-20230415235602744.png)

* If we want to find the 5th element in the array, simply return that pivot.
* If we want to find a k < 5, we again search through the left array
* If want to find a k > 5, we search through the right array with k as k - 5

#### Pseudocode

![image-20230415235741876](attachments/cs161.assets/image-20230415235741876.png)

* The choice of 50 is arbitrary. This base case is because it's a set length, so its O(1). Technically, if you sort a fixed length array, it's 0(1). Big-O is focused on what happens as the array length grows.

#### Proof for why it Works

* Check the iPython notebook and the formal proof on the website.

#### Running Time

![image-20230416124510612](attachments/cs161.assets/image-20230416124510612.png)

![image-20230416124604924](attachments/cs161.assets/image-20230416124604924.png)

#### Picking Pivot

* A good pivot is one that splits the array into half, where the length of $L$ is n/2 and the length of $R$ is n/2.

 ![image-20230416124721789](attachments/cs161.assets/image-20230416124721789.png)

* With this ideal pivot, the recurrence relation is roughly $T(n/2) + O(n)$.

![image-20230416124832304](attachments/cs161.assets/image-20230416124832304.png)

* However, we can't always pick this ideal pivot. Consider the case when we have the worst pivot. This is when we either pick the largest or smallest element.

![image-20230416124949312](attachments/cs161.assets/image-20230416124949312.png)

![image-20230416124954778](attachments/cs161.assets/image-20230416124954778.png)

![image-20230416125040778](attachments/cs161.assets/image-20230416125040778.png)

* The algorithm in that aside is called QuickSelect.
* In our case, let's assume there's a bad guy.

![image-20230416125500222](attachments/cs161.assets/image-20230416125500222.png)

### Approach to Picking Pivot

#### Ideal Pivot

![image-20230416125559156](attachments/cs161.assets/image-20230416125559156.png)

#### Good Enough Pivot

![image-20230416125610098](attachments/cs161.assets/image-20230416125610098.png)

![image-20230416125640805](attachments/cs161.assets/image-20230416125640805.png)

* We pick a pivot that doesn't exactly split it in half, but gets us close to half.
* Instead of n/2 for our recurrence relation, it's 7n/10, which is size of the bigger list.

#### Goal

![image-20230416125816575](attachments/cs161.assets/image-20230416125816575.png)

### Solution

![image-20230416125836463](attachments/cs161.assets/image-20230416125836463.png)

* We recursively find medians on smaller arrays.
* We get out big array, chop it up into smaller arrays, then find median of those smaller arrays.
  * Then, we find the median of those medians.
* The median of sub-medians is good enough.

#### Picking the Pivot

* We first split the array into chunks.

![image-20230416130013866](attachments/cs161.assets/image-20230416130013866.png)

* Then, we find the median of each of those chunks. This is O(1) because the chunks have a set size.

![image-20230416130041086](attachments/cs161.assets/image-20230416130041086.png)

* Then, we recursively call select with just these medians.

![image-20230416130153777](attachments/cs161.assets/image-20230416130153777.png)

* Now we partition around that pivot.

![image-20230416130205838](attachments/cs161.assets/image-20230416130205838.png)

#### Claim

 ![image-20230416130236381](attachments/cs161.assets/image-20230416130236381.png)

![image-20230416130244314](attachments/cs161.assets/image-20230416130244314.png)

![image-20230416130328117](attachments/cs161.assets/image-20230416130328117.png)

#### Running Time

![image-20230416130346505](attachments/cs161.assets/image-20230416130346505.png)

![image-20230416130424996](attachments/cs161.assets/image-20230416130424996.png)

* There's one recursive call in the choosePivot, and another one in either of the cases at the bottom (else if).
* The one in the cases is at most T(7n/10) because that's the max size of L and R.
* Then, the recursive call in choose pivot has size n/5, because we take the median from each of the chunks.
* The amount of work done in the subproblem is O(n) because choosePivot takes O(n), and partition takes O(n).

### Proof (Substitution Method)

![image-20230416130623678](attachments/cs161.assets/image-20230416130623678.png)

### Recap

![image-20230416130703213](attachments/cs161.assets/image-20230416130703213.png)

![image-20230416130717482](attachments/cs161.assets/image-20230416130717482.png)

![image-20230416130743974](attachments/cs161.assets/image-20230416130743974.png)

### Proof of Lemma

![image-20230416130808529](attachments/cs161.assets/image-20230416130808529.png)

![image-20230416130818691](attachments/cs161.assets/image-20230416130818691.png)

![image-20230416130834931](attachments/cs161.assets/image-20230416130834931.png)

![image-20230416130847370](attachments/cs161.assets/image-20230416130847370.png)

![image-20230416130854679](attachments/cs161.assets/image-20230416130854679.png)

![image-20230416130907999](attachments/cs161.assets/image-20230416130907999.png)

![image-20230416130919891](attachments/cs161.assets/image-20230416130919891.png)

![image-20230416130926469](attachments/cs161.assets/image-20230416130926469.png)

![image-20230416130941057](attachments/cs161.assets/image-20230416130941057.png)

![image-20230416130956478](attachments/cs161.assets/image-20230416130956478.png)

---

## 4/17 L5: Randomized Algorithms

### Recap

![image-20230419231649116](attachments/cs161.assets/image-20230419231649116.png)

* Today, we will investigate the random pivot and randomized algorithms.

### Randomized Algorithms

* We make some random choices during the algorithm.
* We hope the algorithm works.
* We hope the algorithm is fast.
* *Today, we focus on algorithms that always work and are probably fast*

![image-20230419231751250](attachments/cs161.assets/image-20230419231751250.png)

### How Do We Measure Runtime of Randomized Algorithm?

![image-20230419232650344](attachments/cs161.assets/image-20230419232650344.png)

* Both scenarios is worst-case analysis.
* In scenario 1, the bad guy picks an input first, then the pivots/flips/random stuff is picked after the input.
* In scenario 2, the bad guy picks an input *and* picks the pivots/flips/random stuff. They get to pick the outcome of the dance for the one that is worst for the algorithm.
  * In this case, they essentially remove the randomness. The bad guy is arranging it deterministically to make it bad.

### BogoSort

![image-20230419234237781](attachments/cs161.assets/image-20230419234237781.png)

* In the above, the $n!$ assumes there are no duplicate elements.

![image-20230419234313934](attachments/cs161.assets/image-20230419234313934.png)

![image-20230419234305488](attachments/cs161.assets/image-20230419234305488.png)

![image-20230419235320067](attachments/cs161.assets/image-20230419235320067.png)

![image-20230419235328681](attachments/cs161.assets/image-20230419235328681.png)

#### Geometric Random Variable

* A GRV is the number of trials you do before you succeed.

* If you are doing a bunch of independent trials that are each successful with a probability $p$, the expected number of trials that you do before you succeed is $1/p$.

#### Expected Running time of BogoSort

![image-20230419235600173](attachments/cs161.assets/image-20230419235600173.png)

#### Worst Case Running time of BogoSort

* The worst case running time is infinite.
* Since it's randomizing the array, the bad guy can just choose the roll so that the list is never sorted.

#### Summary

![image-20230419235835642](attachments/cs161.assets/image-20230419235835642.png)

### QuickSort

![image-20230420113347188](attachments/cs161.assets/image-20230420113347188.png)

* A lot of languages use QuickSort as the default sorting algorithm.

#### How it Works

![image-20230420113632905](attachments/cs161.assets/image-20230420113632905.png)

* We pick a random pivot, then partition it with that pivot.
* Then, we recursively call Quicksort on the left and right half to get a sorted list.

![image-20230420113706270](attachments/cs161.assets/image-20230420113706270.png)

#### Pseudocode

![image-20230420113811624](attachments/cs161.assets/image-20230420113811624.png)

#### Running time

* The partition is $O(n)$.

![image-20230420113849288](attachments/cs161.assets/image-20230420113849288.png)

#### Wrong Proof

* Note that this proof is slightly cheating.

![image-20230420114111979](attachments/cs161.assets/image-20230420114111979.png)

* This is claiming that when we pick a random pivot, the expected number of things on the left half is the same as the expected number of things on the right half.

![image-20230420114048807](attachments/cs161.assets/image-20230420114048807.png)

* Why is this proof wrong?
  * Because we can use the same proof to prove something false.

![image-20230420114234486](attachments/cs161.assets/image-20230420114234486.png)

* The expected size of $L$ and $R$ is $n-1/2$ because based on the pivot, the size of each is either $0$ or $n-1$, each with probability $1/2$ (because the pivot is min or max).

![image-20230420114356076](attachments/cs161.assets/image-20230420114356076.png)

* The issue with the proof above is that that's not how expectations work.

![image-20230420114506412](attachments/cs161.assets/image-20230420114506412.png)

* We're basically saying the running of the expected list sizes is the same as the expected running time with all list sizes. This is not how it works.

#### How Does Quicksort Recurse?

![image-20230420114646346](attachments/cs161.assets/image-20230420114646346.png)

* The elements on each half are separated in each recursion level.

![image-20230420114808018](attachments/cs161.assets/image-20230420114808018.png)

* We can count the number of comparisons instead of operations because, in this case, the number of comparisons dominate the running time (proof in textbook)

![image-20230420115013568](attachments/cs161.assets/image-20230420115013568.png)

* The only time we compare items is when we pick the pivot.
  * When we do this, everything in scope is compared, but everything not in scope is never compared (scope as in which half: ie. 3 and 7 are never compared)

![image-20230420115110511](attachments/cs161.assets/image-20230420115110511.png)

* We treat it as a random variable because we're interested in the expected number of comparisons.

![image-20230420115218321](attachments/cs161.assets/image-20230420115218321.png)

#### Counting Comparisons

![image-20230420115252974](attachments/cs161.assets/image-20230420115252974.png)

* The total number of comparisons is sum over all pairs of elements, whether the two elements were compared.
  * Note that the summation starts $b$ at $a + 1$ to avoid double counting (e.g. counting 2 for when 3 is compared to 6 and when 6 is compared to 3)
* Now we just need to think about $X_{a, b}$ specifically what the probability is that $a$ and $b$ are ever compared to each other.

![image-20230420115444996](attachments/cs161.assets/image-20230420115444996.png)

* Note that $X_{a,b}$ is either $1$ or $0$. Therefore, it's an indicator random variable. As such, its expected value is just the probability that it equals $1$.
* The only way that 2 and 6 can be compared is if one is chosen as the pivot while the other is still in scope.
* The only way that $2$ and $6$ are never compared is if the pivot splits them into different halves. As such, if $3, 4, 5$ are picked first, then they're in different halves and thus never compared.
* If 1 or 7 is picked as the pivot, the analysis is just deferred to a later step since 2 and 6 just go on the same side. They aren't compared yet, but its not impossible for them to be compared either.

![image-20230420120107166](attachments/cs161.assets/image-20230420120107166.png)

* It doesn't affect the probability.

![image-20230420115813801](attachments/cs161.assets/image-20230420115813801.png)

* $b - a + 1$ is all the number of numbers in between $a$ and $b$, assuming the numbers are from $1$ to $7$.

![image-20230420120313238](attachments/cs161.assets/image-20230420120313238.png)

![image-20230420120358699](attachments/cs161.assets/image-20230420120358699.png)

* All the work is happening in the partition step, which takes $O(n)$ work *and* $O(n)$ comparisons.

#### Expected Running Time

![image-20230420120458250](attachments/cs161.assets/image-20230420120458250.png)

#### Worst-case Running Time

![image-20230420120605717](attachments/cs161.assets/image-20230420120605717.png)

* The adversary would choose either min or max of the array, which would make it become $O(n^2)$.
* The expected is $n \log n$ instead of $n^2$ because it's really unlikely that you pick the minimum or maximum element if you pick randomly.

#### Implementation

![image-20230420120840188](attachments/cs161.assets/image-20230420120840188.png)

### Better way to Do Partition

![image-20230420121107806](attachments/cs161.assets/image-20230420121107806.png)

* Initialize two pointers, the red one and the blue one.

![image-20230420121145255](attachments/cs161.assets/image-20230420121145255.png)

* The blue bar was incremented. Then, it finds the $1$, which is less than the pivot. Then, it swaps the elements directly following each of the two bars.

![image-20230420121218957](attachments/cs161.assets/image-20230420121218957.png)

* 3 is less than the pivot, so we swap again, and advance both bars.

![image-20230420121250184](attachments/cs161.assets/image-20230420121250184.png)

* Now we keep advancing the blue bar until we see something smaller than the pivot. It won't find anything smaller, until it reaches the pivot itself.

![image-20230420121333217](attachments/cs161.assets/image-20230420121333217.png)

* Now we again the swap, then advance the bars, and then we're done.

#### Why Does This Work?

* If we prove it by induction, the inductive hypothesis is that
  * Everything between the red and blue bars (colored orange) is greater than the pivot.
  * Everything before the red bar (colored green) is smaller than the pivot.
  * Everything in blue is still unconsidered, or the pivot itself.
* For the inductive step, we would be like advancing the bars and swapping, which also maintains the inductive hypothesis.
* We're gathering everything smaller than pivot before the red bar, everything bigger than the pivot in between the bars, then at the end we swap 4 into the right place.

### QuickSort Vs MergeSort

![image-20230420121658971](attachments/cs161.assets/image-20230420121658971.png)

![image-20230420121736763](attachments/cs161.assets/image-20230420121736763.png)

* Stability refers to the idea that if you have two items with the same key, if one appears before the other in the original list, you want that one to also appear before the other in the sorted list.

### Recap

![image-20230420121946017](attachments/cs161.assets/image-20230420121946017.png)

---

## 4/19 L6: Sorting Lower Bounds, Radix/Bucket

### Sorting

* So far, we have seen a few O(n log n)-time algorithms
  * MERGESORT has worst-case running time of O(nlogn)
  * QUICKSORT has expected running time O(nlogn), and worst case O(n^2)

* This is generally the best we can do. However, if the input has some special conditions, then we can theoretically do better.

### Sorting Models of Computation

![image-20230424124501743](attachments/cs161.assets/image-20230424124501743.png)

### Comparison-based Sorting

* You want to sort an array of items.
* In comparison-based, you can't access the items' values directly; you can only compare two items and find out which is bigger or smaller.

![image-20230424124651861](attachments/cs161.assets/image-20230424124651861.png)

![image-20230424124724873](attachments/cs161.assets/image-20230424124724873.png)

* All the sorting algorithms we know so far can only access values in this comparison-based way.

![image-20230424124822255](attachments/cs161.assets/image-20230424124822255.png)

* As an example, in quick sort we pick a pivot. Then, we go and compare every other item to that pivot.

![image-20230424124904741](attachments/cs161.assets/image-20230424124904741.png)

### Theorem

![image-20230424125006494](attachments/cs161.assets/image-20230424125006494.png)

### Decision Trees

* Suppose we want to sort three things. It may give rise to a decision tree like below.

![image-20230424125111646](attachments/cs161.assets/image-20230424125111646.png)

* At each node, we essentially ask the genie a comparison question to find the ordering.

![image-20230424125156618](attachments/cs161.assets/image-20230424125156618.png)

![image-20230424125220800](attachments/cs161.assets/image-20230424125220800.png)

#### Runtime on an Input

![image-20230424125422774](attachments/cs161.assets/image-20230424125422774.png)

* The running time is at least the length the path for that input.
  * In this case, the running time is determined by the number of comparisons.

#### Runtime in Worst case

![image-20230424125512506](attachments/cs161.assets/image-20230424125512506.png)

* The worst-case running time is the length of the longest path. This longest path is the depth of the tree.

![image-20230424125550959](attachments/cs161.assets/image-20230424125550959.png)

* There are at least $n!$ leaves, because there are $n!$ possible orderings that we could output for our array.
  * We say at least because we don't know what the algorithm is doing. There could possibly be duplicated orderings.
* The longest path of every tree is at least $\log(n!)$.
  * This is because at level 1, there is 2 nodes. At level 2, there is 4 nodes. At level $j$, there is $2^j$ nodes. If we let $j = \log(n!)$, we have that level $j$ has $n!$ nodes. This is the last level, hence our depth is $\log(n!)$.

### Proof Recap

![image-20230424130340725](attachments/cs161.assets/image-20230424130340725.png)

![image-20230424130446202](attachments/cs161.assets/image-20230424130446202.png)

![image-20230424130453106](attachments/cs161.assets/image-20230424130453106.png)

### Faster than nlog(n)

![image-20230424130537256](attachments/cs161.assets/image-20230424130537256.png)

* In this model of computation, our values actually have meaningful values. We're looking at the values themselves now instead of looking at how it compares to others.

![image-20230424130626431](attachments/cs161.assets/image-20230424130626431.png)

* With values that have meaning, we can sort them quicker. A good example is sorting by month.
  * We can separate people into buckets based on their birth month, and then simply combine everyone back together. This would take $O(n)$ time.

### CountingSort

![image-20230424130900900](attachments/cs161.assets/image-20230424130900900.png)

* With CountingSort, we initialize the nine buckets. Then, we iterate through the array and put the items in the bucket they belong, one by one. Then, we simply read them out of the buckets.

#### Assumptions

![image-20230424131017786](attachments/cs161.assets/image-20230424131017786.png)

* We need to know what shows up ahead of time so that we can make the appropriate buckets.

### RadixSort

![image-20230424131129618](attachments/cs161.assets/image-20230424131129618.png)

* You can use RadixSort to sort both integers and strings. It can use less space because it uses less buckets.

#### Step 1: CountingSort on Least Significant Digit

![image-20230424131214852](attachments/cs161.assets/image-20230424131214852.png)

* After we put them in the buckets, we take them all out and concatenate them in order. Note that the buckets are FIFO queues.

#### Step 2: CountingSort on Second Least Sig. Digit

![image-20230424131304221](attachments/cs161.assets/image-20230424131304221.png)

* For numbers without the 2nd least sig. digit, like $1$, we can imagine that we just pad it with zeros. As such, we put it in the zero bucket.
* Importantly again, we create the final array by taking numbers out in FIFO order.

#### Step 3: CountingSort on the Third Least Sig. Digit

![image-20230424131353443](attachments/cs161.assets/image-20230424131353443.png)

#### Why Does it Work?

![image-20230424131431150](attachments/cs161.assets/image-20230424131431150.png)

### Proof by Induction
![image-20230424131637969](attachments/cs161.assets/image-20230424131637969.png)

* To prove this, we'll use induction. The inductive hypothesis is that after iteration $i$, the array is sorted by the $i$ least significant digits.

![image-20230424131709659](attachments/cs161.assets/image-20230424131709659.png)

* For the base case, we want to show that it's sorted by the first 0 least-significant digits.  This is vacuously true. This means its not necessarily sorted, so that holds, since our array is unsorted originally.

#### Inductive step

![image-20230424142233289](attachments/cs161.assets/image-20230424142233289.png)

#### Proof Sketch

* Suppose we have two elements $x$ and $y$, both of which are $d$ digits numbers. We represent the digits as an array.
* Now suppose that the i least-sig digits of x are less than that of y.

![image-20230424142431226](attachments/cs161.assets/image-20230424142431226.png)

![image-20230424142541215](attachments/cs161.assets/image-20230424142541215.png)

* In the case above, we know that the array is sorted by the first digit. Now, since this next digit is the same, we know they will be in the same bucket. However, we want x to be in the bucket first so that it is taken out first, so that it is earlier in the resulting array.

![image-20230424142832948](attachments/cs161.assets/image-20230424142832948.png)

* Notice that there is no $x_i > y_i$ case because of the second bullet point.
* Thus, with this, we know that RadixSort is correct.

### Running Time of RadixSort
![image-20230424143346785](attachments/cs161.assets/image-20230424143346785.png)

![image-20230424143420948](attachments/cs161.assets/image-20230424143420948.png)

* Notice that the above is doing RadixSort in base 10. It seems to be performing just like MergeSort.
* However, suppose we now change the base.

![image-20230424143617954](attachments/cs161.assets/image-20230424143617954.png)

#### Example Base Change

![image-20230424143637687](attachments/cs161.assets/image-20230424143637687.png)

* First we pad it with zeros
![image-20230424143651257](attachments/cs161.assets/image-20230424143651257.png)

* In the first iteration above, we look at the two least significant digits (because its base 100)
![image-20230424143742506](attachments/cs161.assets/image-20230424143742506.png)

* After just two iterations, it's already sorted.
![image-20230424143813999](attachments/cs161.assets/image-20230424143813999.png)

#### General Running time
![image-20230424143836807](attachments/cs161.assets/image-20230424143836807.png)

* If we let $r = M$, then we essentially have counting sort.
* In a general case, what base would be good?

![image-20230424144017125](attachments/cs161.assets/image-20230424144017125.png)

![image-20230424144032926](attachments/cs161.assets/image-20230424144032926.png)

### Recap
![image-20230424144115348](attachments/cs161.assets/image-20230424144115348.png)

![image-20230424144153813](attachments/cs161.assets/image-20230424144153813.png)

---

## 4/24 L7: BST and Red-Black Tress

### Recap of what We Have so far

![image-20230424150336336](attachments/cs161.assets/image-20230424150336336.png)

![image-20230424150631145](attachments/cs161.assets/image-20230424150631145.png)

![image-20230424150643338](attachments/cs161.assets/image-20230424150643338.png)

![image-20230424151253206](attachments/cs161.assets/image-20230424151253206.png)

![image-20230424151303171](attachments/cs161.assets/image-20230424151303171.png)

* Left of the pivot is less than, right of the pivot is greater than

![image-20230424151458952](attachments/cs161.assets/image-20230424151458952.png)

### Data Structures

* We know a few ways to store objects. In this case, we focus on objects that are nodes with a key.

![image-20230424151812430](attachments/cs161.assets/image-20230424151812430.png)

* In the above, the node is the blue box, and the key is the $5$.

![image-20230424151913248](attachments/cs161.assets/image-20230424151913248.png)

* Sorted arrays have an $O(n)$ time insert/delete, and an $O(\log n)$ time search.

![image-20230424151958399](attachments/cs161.assets/image-20230424151958399.png)

### BSTs

![image-20230424152034404](attachments/cs161.assets/image-20230424152034404.png)

#### Binary Tree

![image-20230424152059024](attachments/cs161.assets/image-20230424152059024.png)

* In the above, note that $5$ is an ancestor of $2$.

#### Binary Search Trees

![image-20230424152306183](attachments/cs161.assets/image-20230424152306183.png)

* The process of building it is as follows. First, we pick an arbitrary root node.

  ![image-20230424152331821](attachments/cs161.assets/image-20230424152331821.png)

* Then, we partition elements around that root node and pick arbitrary children.

  ![image-20230424152410475](attachments/cs161.assets/image-20230424152410475.png)

* Then, we partition elements around those children and pick more things to be children.

  ![image-20230424152441805](attachments/cs161.assets/image-20230424152441805.png)

* We again partition around these, and then we get our resulting tree.

  ![image-20230424152500054](attachments/cs161.assets/image-20230424152500054.png)

* Notice that this process is kind of like QuickSort with all the partitions.

![image-20230424152538196](attachments/cs161.assets/image-20230424152538196.png)

### In-order Traversal

![image-20230424152612805](attachments/cs161.assets/image-20230424152612805.png)

### BST Operations

![image-20230424154141146](attachments/cs161.assets/image-20230424154141146.png)

![image-20230424154243567](attachments/cs161.assets/image-20230424154243567.png)

![image-20230424154258773](attachments/cs161.assets/image-20230424154258773.png)

![image-20230424154415097](attachments/cs161.assets/image-20230424154415097.png)

* For a balanced tree, SEARCH will take $O(\log n)$.

![image-20230424154449709](attachments/cs161.assets/image-20230424154449709.png)

* However, notice above that the tree is very unbalanced, so the SEARCH might devolve to $O(n)$.

![image-20230424154519701](attachments/cs161.assets/image-20230424154519701.png)

* With $\mathtt{Idea 0}$, it's not a good idea because we'll need to rebuild it often if we're constantly working with it, which can end up taking a lot of time.

### Self-Balancing BSTs

* There are many ways to automatically balance a binary search tree.

#### Idea 1: Rotations

![image-20230424154739783](attachments/cs161.assets/image-20230424154739783.png)

* This rotation process is done mostly by pointer manipulation, which is why it's O(1).

![image-20230424154914385](attachments/cs161.assets/image-20230424154914385.png)

![image-20230424154923296](attachments/cs161.assets/image-20230424154923296.png)

#### Idea 2: Have Some Proxy for Balance

* Maintaining perfect balance is hard. As such, we come up with some property to show that it's *pretty* balanced.

![image-20230424154956928](attachments/cs161.assets/image-20230424154956928.png)

### Red-Black Trees

![image-20230424160442724](attachments/cs161.assets/image-20230424160442724.png)

![image-20230424160453561](attachments/cs161.assets/image-20230424160453561.png)

* If you start at 5 above, every path to get to NIL has three black nodes.
* If you start from 3, every path to get to NIL has two black nodes.

#### Examples

![image-20230424160955785](attachments/cs161.assets/image-20230424160955785.png)

* For the left one, the root node is not black.
* For the middle one, the children of the red node is not black.
* For the right one:
  * If we go to the left from the root, there are three black nodes (including NIL).
  * If we go to the right of the root to the red, then right again to the NIL, there are two black nodes (including NIL).

#### Why These Rules?

![image-20230424161122719](attachments/cs161.assets/image-20230424161122719.png)

* This is balanced enough to have $O(\log n)$ depth, but can actually be maintained.

---

#### Red Black Tree

![image-20230501123432154](attachments/cs161.assets/image-20230501123432154.png)

![image-20230501123806320](attachments/cs161.assets/image-20230501123806320.png)

* Notice that $b(x)$ applies to all paths from $x$, because of the definition of a red-black tree.
* The claim is that in the whole green blob area, there are at least $2^{b(x)} - 1$ nodes.

---

#### Proof

Let $d(x)$ be the number of non-NIL nodes in the subtree under $x$, including $x$ itself. We want to show that $d(x) \geq 2^{b(x)} -1$.

**Inductive Hypothesis**: The CLAIM above is true for all RB trees of height $k$

**Base Case**: Consider the case when $k = 0$. In this case, the tree is essentially just our node $x$ with no children (technically two NIL children). In this case, $d(x) = 1$ because $x$ is the only node (excluding NIL). In addition, $b(x) = 1$ because in any path, there is 1 black node, the NIL child. Now, notice that
$$
\begin{equation*}
	2^{b(x)} - 1 = 2 - 1 = 1 = d(x)
\end{equation*}
$$
**Inductive Step**: Assume the IH holds for $k = h$. We want to show that it holds for $k = h + 1$.

<img src="./cs161.assets/image-20230501124540829.png" alt="image-20230501124540829" style="zoom:150%;" />

In the case above, notice that $d(x) = 1 + d(y) + d(z)$. Since $y$ and $z$ are RB trees of height $h$, by our IH, we know that
$$
\begin{align*}
	d(x) &\geq 1 + (2^{b(y)} -1) + (2^{b(z)} - 1) \\
	d(x) &\geq 2^{b(y)} + 2^{b(z)} - 1
\end{align*}
$$
*Aside:* We claim that $b(x) = b(y)$ if $y$ is red, and $b(x) = b(y) + 1$ if $y$ is black. This is because $b(x)$ is the number of black nodes in any path from $x$ to NIL. This implies that $b(y) \geq b(x) - 1$. The same is true for $b(z)$. We can now use this to plug this in to get
$$
\begin{align*}
	d(x) &\geq 2^{b(y)} + 2^{b(z)} - 1 \\
	 &\geq 2^{b(x) - 1} + 2^{b(x) - 1} - 1 \\
	 &\geq 2 * 2^{b(x) - 1} - 1 \\
	 &\geq 2^{b(x)} - 1
\end{align*}
$$
This is what we wanted to show.

**Conclusion:** As such, the claim holds for any red black tree of any height $k$, so it holds for all red black trees.

![image-20230501125528920](attachments/cs161.assets/image-20230501125528920.png)

* Black nodes can appear at every other height at most, so that's why $b(root)$ is at least height/2.

---

![image-20230501125721764](attachments/cs161.assets/image-20230501125721764.png)

---

#### Insert/Delete

![image-20230501125744465](attachments/cs161.assets/image-20230501125744465.png)

---

### Recap

![image-20230501130002177](attachments/cs161.assets/image-20230501130002177.png)

![image-20230501130016948](attachments/cs161.assets/image-20230501130016948.png)

---

## 4/26 L8: Hashing

![image-20230501134258727](attachments/cs161.assets/image-20230501134258727.png)

* We'll have $O(1)$ *expected* running time for insert/delete/search. The tradeoff is that we'll have a worse worst-case running time.

![image-20230501150325591](attachments/cs161.assets/image-20230501150325591.png)

---

### First Approach at O(1)

* We can do this by creating a bunch of buckets for each item.

![image-20230501150411140](attachments/cs161.assets/image-20230501150411140.png)

![image-20230501150423927](attachments/cs161.assets/image-20230501150423927.png)

* The universe refers to the universe of keys. Essentially, our keys come from a really really big set.

#### Solution

* We can fix this problem by doing it like RadixSort, putting it in buckets based on digits.

![image-20230501150531730](attachments/cs161.assets/image-20230501150531730.png)

* This is slightly faster. However, it devolves to $O(n)$ if all the elements are in the same bucket.

---

### Hash Tables

![image-20230501150753043](attachments/cs161.assets/image-20230501150753043.png)

---

#### Terminology

![image-20230501151357384](attachments/cs161.assets/image-20230501151357384.png)

* There are 128 ascii characters, so there are $128^{280}$ possible 280 character strings.

---

#### Hash Functions

* This function maps elements from $U$ (our keys) to buckets

![image-20230501151850405](attachments/cs161.assets/image-20230501151850405.png)

---

#### Hash Tables

![image-20230501152021879](attachments/cs161.assets/image-20230501152021879.png)

* In the picture above, 43 should go before 13 because we'd be inserting by inserting it at the head.

![image-20230501152235894](attachments/cs161.assets/image-20230501152235894.png)

---

### Hash Families

![image-20230501152947540](attachments/cs161.assets/image-20230501152947540.png)

#### Worst Case Analysis

![image-20230501153058979](attachments/cs161.assets/image-20230501153058979.png)

![image-20230501153346112](attachments/cs161.assets/image-20230501153346112.png)

* We can't get $O(1)$ time if we use a deterministic hash function, since a bad guy will always be able to thwart it.

![image-20230501153450157](attachments/cs161.assets/image-20230501153450157.png)

* Suppose the universe gets split into areas, where each of those gets mapped to a specific bucket.
* By the Pigeonhole Principle, at least one bucket will have $M/n$ items hashed into it.

---

### Randomized Hash Functions

![image-20230501153704985](attachments/cs161.assets/image-20230501153704985.png)

* Darth vader takes any $n$ items from the universes (the green blob) and picks a sequence of operations.

<img src="./cs161.assets/image-20230501153757531.png" alt="image-20230501153757531" style="zoom:33%;" />

* Our goal is to show that these chains at the bottom don't get *too* long.

#### Example: Uniformly Random Function

![image-20230501153903751](attachments/cs161.assets/image-20230501153903751.png)

![image-20230501154005664](attachments/cs161.assets/image-20230501154005664.png)

* $u_i$ is just one of the keys.

![image-20230501154036428](attachments/cs161.assets/image-20230501154036428.png)

* The time of operation is $O(1)$ because $h(u)$ is $O(1)$, and then the chain is a constant number so searching through it is also $O(1)$.

![image-20230501154157140](attachments/cs161.assets/image-20230501154157140.png)

---

#### Aside from Pre-lecture 8

* Note that the expected number of items in bucket $i$ is different from the expected number of items in $u_i$'s bucket.
* Suppose we have a random roll from $1$ to $n$, and if it lands on $i$, then all the items goes into bucket $i$.

![image-20230501154344432](attachments/cs161.assets/image-20230501154344432.png)

![image-20230501155128826](attachments/cs161.assets/image-20230501155128826.png)

---

![image-20230501155159785](attachments/cs161.assets/image-20230501155159785.png)

![image-20230501155227975](attachments/cs161.assets/image-20230501155227975.png)

* We wrote the quantity that we're interested as a sum of indicator random variables. The above means it's $1$ if $h(u_i) = h(u_j)$, otherwise its $0$.

![image-20230501155315832](attachments/cs161.assets/image-20230501155315832.png)

* We now use linearity of expectation with the equation above. The expectation of an indicator random variable is just its probability of success (ie. prob that it equals 1)

![image-20230501155401026](attachments/cs161.assets/image-20230501155401026.png)

* We pull out the $1$ because the probability of $h(u_i) = h(u_i)$ is just $1$, so we can simply pull it out.

![image-20230501155453837](attachments/cs161.assets/image-20230501155453837.png)

* With a uniformly random hash function, the probability that $u_i$ and $u_j$ land in the same bucket is $1/n$.
  * Imagine $u_i$ lands in some bucket. Now there's $n$ different places that $u_j$ could land, but only one of them is where $u_i$ landed. Therefore, the probability that it lands in that spot is $1/n$. (to formally prove, will need to explicitly use the fact that they are independent)

![image-20230501155646920](attachments/cs161.assets/image-20230501155646920.png)

* The above equation is derived from the fact that there are $n$ possible $j$'s, but we are leaving out $j = i$, so its $n-1$ possible $j$'s.

---

![image-20230501155733172](attachments/cs161.assets/image-20230501155733172.png)

![image-20230501160238151](attachments/cs161.assets/image-20230501160238151.png)

* Note that this function picks $h(i)$ as uniformly random then saves that that for the rest of time. As such, we don't have the issue where we have to recalculate and search for $h(i)$ each time we pass $i$.
* The issues with this is that we need to store the location of the each of those $n$ numbers. We need to remember what $h(i)$ is, and we can't just recalculate it.

![image-20230501225046315](attachments/cs161.assets/image-20230501225046315.png)

* It needs to store the value of each of those $M$ things in the universe.

![image-20230501230613292](attachments/cs161.assets/image-20230501230613292.png)

* Hash functions go from $U$ to the positions. There are $n^M$ possible hash functions. With a random function, we need to store $\log(n^M)$ bits.

---

### Hash Family

![image-20230501231201724](attachments/cs161.assets/image-20230501231201724.png)

* A hash family is a collection of hash functions.

![image-20230501231327539](attachments/cs161.assets/image-20230501231327539.png)

![image-20230501231405770](attachments/cs161.assets/image-20230501231405770.png)

* Choose a smaller hash family can let us save space.

---

### Universal Hash Family

![image-20230501231519461](attachments/cs161.assets/image-20230501231519461.png)

![image-20230501231528824](attachments/cs161.assets/image-20230501231528824.png)

![image-20230501231537429](attachments/cs161.assets/image-20230501231537429.png)

![image-20230501231611465](attachments/cs161.assets/image-20230501231611465.png)

![image-20230501231647136](attachments/cs161.assets/image-20230501231647136.png)

#### Example

![image-20230501231656419](attachments/cs161.assets/image-20230501231656419.png)

![image-20230501231820341](attachments/cs161.assets/image-20230501231820341.png)

![image-20230501231901653](attachments/cs161.assets/image-20230501231901653.png)

* The number of hash functions in $H$ is the number of pairs $a$ and $b$. As such, $|H| = p^2 - p = O(M^2)$.

---

### Recap of Hash Tables

#### Hash Table Overview

![image-20230501232917189](attachments/cs161.assets/image-20230501232917189.png)

#### How Do We Pick the Hash Function?

![image-20230501233008517](attachments/cs161.assets/image-20230501233008517.png)

* The uniformly random function is bad because we can't really store it efficiently.

#### Universal Hash Families

![image-20230501233054744](attachments/cs161.assets/image-20230501233054744.png)

* The above is essentially the probability of collision.
* We essentially reduce the randomness a little, but maintain the collision property.

#### Example

![image-20230501233228480](attachments/cs161.assets/image-20230501233228480.png)

#### Setup: Space

![image-20230501233303664](attachments/cs161.assets/image-20230501233303664.png)

* a and b is picked once, and it's no longer picked again.

#### Setup: Time

![image-20230501233434521](attachments/cs161.assets/image-20230501233434521.png)

---

### Conclusion

![image-20230501232146333](attachments/cs161.assets/image-20230501232146333.png)

* It has worst case $O(1)$ insert, and worst case $O(n)$ delete and search.

---

## 5/1 L8.5: Midterm Review

### Lecture 1

![image-20230501234230904](attachments/cs161.assets/image-20230501234230904.png)

* We take a big problem and we break it into small problems, often recursively.
* As a comparison, the grade-school multiplication ran in time $O(n^2)$

### Lecture 2

![image-20230501234312593](attachments/cs161.assets/image-20230501234312593.png)

* We talked about what it means to be fast and what it means to work.
* The worst-case analysis guarantees that our algorithm works.

![image-20230501234357858](attachments/cs161.assets/image-20230501234357858.png)

![image-20230501234434027](attachments/cs161.assets/image-20230501234434027.png)

![image-20230501234457223](attachments/cs161.assets/image-20230501234457223.png)

### Lecture 3

![image-20230501234513421](attachments/cs161.assets/image-20230501234513421.png)

* The master theorem is a useful result of the generalized tree method.

![image-20230501234537016](attachments/cs161.assets/image-20230501234537016.png)

![image-20230501234550225](attachments/cs161.assets/image-20230501234550225.png)

### Lecture 4

![image-20230501234711814](attachments/cs161.assets/image-20230501234711814.png)

* To find the pivot, we broke our array into chunks, we found the sub-median of the chunks, then we recursively found the media of the sub-medians.

![image-20230501234828144](attachments/cs161.assets/image-20230501234828144.png)

![image-20230501234837347](attachments/cs161.assets/image-20230501234837347.png)

### Lecture 5

![image-20230501234939151](attachments/cs161.assets/image-20230501234939151.png)

![image-20230501235005987](attachments/cs161.assets/image-20230501235005987.png)

### Lecture 6

![image-20230501235033569](attachments/cs161.assets/image-20230501235033569.png)

* It has $n!$ leaves because there are $n!$ possible orderings to get it sorted.

![image-20230501235059048](attachments/cs161.assets/image-20230501235059048.png)

### Lecture 7

![image-20230501235154665](attachments/cs161.assets/image-20230501235154665.png)

![image-20230501235204267](attachments/cs161.assets/image-20230501235204267.png)

---

## 5/3 L9: Graphs, BFS, and DFS

---

### Graphs

* Graphs have nodes and edges between nodes.

![image-20230508131124453](attachments/cs161.assets/image-20230508131124453.png)

---

### Undirected Graphs

* Graphs where the edges have no direction.

![image-20230508131217732](attachments/cs161.assets/image-20230508131217732.png)

* We use the brackets to denote that its an unordered set.

### Directed Graph

![image-20230508131247155](attachments/cs161.assets/image-20230508131247155.png)

* The normal parentheses denote that the edges are ordered.

---

### How to Represent Graphs

#### Adjacency Matrix

* We put a 1 if there is an edge between two vertices.

![image-20230508131359985](attachments/cs161.assets/image-20230508131359985.png)

* In undirected graphs, the adjacency matrix is symmetric.

![image-20230508131423572](attachments/cs161.assets/image-20230508131423572.png)

![image-20230508131429471](attachments/cs161.assets/image-20230508131429471.png)

---

#### Adjacency List

* Represent as an array of linked lists, where the things in the list is the neighbors of a node.

![image-20230508131458232](attachments/cs161.assets/image-20230508131458232.png)

---

#### Summary

![image-20230508131534980](attachments/cs161.assets/image-20230508131534980.png)

![image-20230508131546930](attachments/cs161.assets/image-20230508131546930.png)

**Adjacency Matrix**

* For the edge membership, simply check `mat[v][w]` to see if it's there.
* To find the neighbors, go through the row and go find the where there is a `1`.

**Adjacency List**

* For the edge membership, we just go through the neighbors of either $v$ or $w$ and find if the other is a neighbor.
* We need to store $n$ buckets and $2m$ edges, so that's the space complexity.

---

### Depth First Search

* This answers the question how we explore a graph?

* At each node, you can get a list of neighbors, and you choose which neighbor to go to.

![image-20230508132346001](attachments/cs161.assets/image-20230508132346001.png)

* Look at the lecture 9 slides to see more.

* Once we hit a green dot, we backtrack and go to another path.

![image-20230508132425408](attachments/cs161.assets/image-20230508132425408.png)

![image-20230508132437197](attachments/cs161.assets/image-20230508132437197.png)

---

![image-20230508132459772](attachments/cs161.assets/image-20230508132459772.png)

* The time stuff isn't necessary for it to work, but it's helpful for analysis.

![image-20230508132626926](attachments/cs161.assets/image-20230508132626926.png)

#### Connected Component

![image-20230508132656796](attachments/cs161.assets/image-20230508132656796.png)

* Consider the graph above, which is two connected components. To explore it, we'd have to run DFS multiple times, on each component.

#### Why Depth-first?

* Consider it as building a tree.

![image-20230508132826017](attachments/cs161.assets/image-20230508132826017.png)

* We ignore the edges that we didn't walk down (the blue edges)

#### Running Time

* Since we can think of it as a tree, we can think of it using the running time of graphs.

![image-20230508132926173](attachments/cs161.assets/image-20230508132926173.png)

![image-20230508132942988](attachments/cs161.assets/image-20230508132942988.png)

* The time at each vertex is $O(1)$ per neighbor, for $deg(W)$ neighbors, so it's $O(deg(W))$.
* The sum over all $w \in V$ of the degree of $w$ is basically just the number of edges.

#### Directed Graph

* DFS also works for directed graphs. Just simply only go through the edges that actually exist.

---

### Topological Sorting

* This is an application of DFS

* [Topological Sort Algorithm | Graph Theory - YouTube](https://www.youtube.com/watch?v=eL-KzMXSXXI)

![image-20230510153141003](attachments/cs161.assets/image-20230510153141003.png)

![image-20230510153127583](attachments/cs161.assets/image-20230510153127583.png)

* A topological ordering is essentially aligning all the nodes in the line, and having all the edges point to the right
  * Going from left to right, no edge points backwards

![image-20230510153324303](attachments/cs161.assets/image-20230510153324303.png)

#### Packages

* Suppose we wanted to install packages without violating dependency requirements.

![image-20230508133809693](attachments/cs161.assets/image-20230508133809693.png)

* This basically says you need to install dpkg first before you can install lbbz2.
* Suppose we run DFS on the graph above, keeping track of the start and end times.

![image-20230508133940039](attachments/cs161.assets/image-20230508133940039.png)

* We went from dpkg to lbbz2, then to lbselinux1, then to multiarch-support, then we backtracked until we got to dpkg. Then, we went to tar, then backtracked, then went to coreutils, then we backtracked.
  * To backtrack on DFS with directed graphs, just go back the way you came from.

#### Finish Times Theorem

* For toposort, we focus on the finish times.

  * At a high level, we run DFS, and then return the finishing orders in decreasing order.

  ![image-20230508134559459](attachments/cs161.assets/image-20230508134559459.png)

  ![image-20230508134621331](attachments/cs161.assets/image-20230508134621331.png)

  * Remember that with DFS, it fully explores the tree before going back up.

  ![image-20230508134731927](attachments/cs161.assets/image-20230508134731927.png)

  ![image-20230508134749382](attachments/cs161.assets/image-20230508134749382.png)

  ![image-20230508134851576](attachments/cs161.assets/image-20230508134851576.png)

#### Topological Sort

![image-20230508134914274](attachments/cs161.assets/image-20230508134914274.png)

![image-20230508134922766](attachments/cs161.assets/image-20230508134922766.png)

* The list on the right there is now in the correct order, without violating any dependence requirements.

![image-20230508135011811](attachments/cs161.assets/image-20230508135011811.png)

---

### Breadth First Search

* Depth first search is like exploring with a birds eye view.

![image-20230508135511964](attachments/cs161.assets/image-20230508135511964.png)

![image-20230508135548792](attachments/cs161.assets/image-20230508135548792.png)

* At the beginning, we essentially make $n$ empty sets.
* You can also use BFS to find connected components, just like with DFS.

#### Running Time

![image-20230508135729258](attachments/cs161.assets/image-20230508135729258.png)

![image-20230508135741166](attachments/cs161.assets/image-20230508135741166.png)

* Each level of the tree corresponds to one of the sets $L_i$.

#### Bacon Numbers Example

![image-20230508135927766](attachments/cs161.assets/image-20230508135927766.png)

* SLJ has a bacon number of two.

![image-20230508135937367](attachments/cs161.assets/image-20230508135937367.png)

* Oliver Sacks has a bacon number of three.

![image-20230508135959831](attachments/cs161.assets/image-20230508135959831.png)

---

#### Distance between W and All other Vertices V

![image-20230508140044002](attachments/cs161.assets/image-20230508140044002.png)

---

#### Recap

![image-20230508140136009](attachments/cs161.assets/image-20230508140136009.png)

* It's $O(m)$ because we're only going through the edges. We only go through one path of vertices.

---

### Application of BFS: Testing Bipartite-ness

![image-20230508140244997](attachments/cs161.assets/image-20230508140244997.png)

### Bipartite Graphs

![image-20230508140309343](attachments/cs161.assets/image-20230508140309343.png)

* Using the idea above, if you put all the red fish in tank A, and the yellow fish in tank B, there will be no fights because there is no paths between the fish of the same color.

![image-20230508140513318](attachments/cs161.assets/image-20230508140513318.png)

![image-20230508140544335](attachments/cs161.assets/image-20230508140544335.png)

![image-20230508140606927](attachments/cs161.assets/image-20230508140606927.png)

* To do this check, while we're checking of the neighboring nodes are unvisited, we can also check if they're the same color.

#### Proof of why This Works

* Just because this coloring doesn’t  work, why does that mean that  there is no coloring that works?

![image-20230508140858371](attachments/cs161.assets/image-20230508140858371.png)

* The edge between $C$ and $D$ makes the highlighted edges a cycle of odd length.

![image-20230508140959140](attachments/cs161.assets/image-20230508140959140.png)

![image-20230508141007467](attachments/cs161.assets/image-20230508141007467.png)

---

### Recap

![image-20230508141021030](attachments/cs161.assets/image-20230508141021030.png)

---

## 5/8 L10: Finding Strongly Connected Components

### Recap

* The key of the algorithms introduced last lecture was paying attention to the structure of the tree that the search algorithms implicitly built.
  * BFS tree, DFS tree, etc

### Recap on DFS

![image-20230510142929264](attachments/cs161.assets/image-20230510142929264.png)

![image-20230510143301241](attachments/cs161.assets/image-20230510143301241.png)

#### Start and End Times

* Recall that we also keep track of the start and finish time for every node.
  * The start time is when we first reach it. The finish time is when all the outgoing from it are explored.
* After we mark something as green, we backtrack and explore the next thing.

![image-20230510143627005](attachments/cs161.assets/image-20230510143627005.png)

#### DFS Tree

![image-20230510143701386](attachments/cs161.assets/image-20230510143701386.png)

* The orange edges form the tree. The edges that we don't take don't show up in the DFS tree.

### Connected Components

* Suppose we have a graph where DFS can't reach every node.

![image-20230510143950440](attachments/cs161.assets/image-20230510143950440.png)

* Notice that $I, J, H$ are unreachable if we start from $A$.

* To fix this, we just run DFS repeatedly to get a **depth-first forest**

  * The DFS forest is made up of DFS trees.

  * In this case, we run DFS again starting from $H$

    ![image-20230510144100560](attachments/cs161.assets/image-20230510144100560.png)

![image-20230510144203814](attachments/cs161.assets/image-20230510144203814.png)

![image-20230510144211579](attachments/cs161.assets/image-20230510144211579.png)

* Notice that if we began with $H$, we could've also gotten $F$ in the right tree.
  * In this case, we did that after. $F$ was already marked as done, so $H$ did not go there.

### Lemma with DFS Tree and Times

![image-20230510144352322](attachments/cs161.assets/image-20230510144352322.png)

### Strongly Connected Components

#### Strongly Connected Graphs

![image-20230510144504870](attachments/cs161.assets/image-20230510144504870.png)

* In the right one, if we started at the center vertex, we can't get to the left vertex.

#### Strongly Connected Components

![image-20230510144811718](attachments/cs161.assets/image-20230510144811718.png)

* Components where every node within that component is mutually reachable.
  * We break the graph into chunks that are strongly connected.

### Why Do We Care about SCCs?

![image-20230510145121542](attachments/cs161.assets/image-20230510145121542.png)

* An arrow means that website links to the other website. In this case, Berkeley links to Stanford, but Stanford doesn't link back.

![image-20230510145158199](attachments/cs161.assets/image-20230510145158199.png)

* Above are the SCCs of the graph, ignoring the disconnected part.

![image-20230510145329446](attachments/cs161.assets/image-20230510145329446.png)

* Many graph algorithms require it to be possible to reach all the nodes.

### How to Find SCCs?

![image-20230510145441623](attachments/cs161.assets/image-20230510145441623.png)

#### Straightforward way to Use DFS to Find SCCs

![image-20230510145629325](attachments/cs161.assets/image-20230510145629325.png)

### Kosaraju's Algorithm

#### Pre-lecture Exercise

![image-20230510145830471](attachments/cs161.assets/image-20230510145830471.png)

* If we run DFS from A, we can get to everything.
* If we run DFS from D, we can only get to DEF.
* The strongly connected components here are ABC and DEF.
* **Suggested Algorithm:** run DFS from the "right" place to identify SCCs
  * What is the right place?
    * If we ran it from A, we get everything, and that doesn't really help.
    * If we ran it from D, we could find that DEF is a SCC.

---

#### Algorithm

* This algorithm has a running time of $O(n + m)$

![image-20230510150048851](attachments/cs161.assets/image-20230510150048851.png)

* In the reverse step, if we had an edge from $A$ to $B$, it now goes from $B$ to $A$.
* For the third step:
  * We start our second run of DFS from the node that has the largest finishing time from the first DFS run. We run it for a while until we stop.
  * To restart, we choose the node with the next largest finishing time.

---

#### Example Run of the Algorithm

1. Here is the strongly connected components of the graph from before. This is what we try to find.

![image-20230510150355937](attachments/cs161.assets/image-20230510150355937.png)

2. Below, we ran DFS on a random node and ran it until completion. However, notice that not all nodes have been visited.

![image-20230510150442498](attachments/cs161.assets/image-20230510150442498.png)

3. We randomly choose an unvisited node and run DFS on it.

![image-20230510150502076](attachments/cs161.assets/image-20230510150502076.png)

4. Now, we reverse all the edges of the graph.

![image-20230510150516131](attachments/cs161.assets/image-20230510150516131.png)

5. Now we mark everything as unvisited, and run DFS again. This time, we start with the node with the largest finishing time (which is Berkeley)

![image-20230510150551029](attachments/cs161.assets/image-20230510150551029.png)

6. We run DFS again, now starting with the next largest finishing time (Stanford)
   1. Things marked gray are already done. They are no longer considered.

![image-20230510150623964](attachments/cs161.assets/image-20230510150623964.png)

7. Finally, we run it again to get our last SCC

![image-20230510150705137](attachments/cs161.assets/image-20230510150705137.png)

---

#### How to Reverse the Edge?

* With an adjacency matrix, simply take the transpose of the adjacency matrix.
* With an adjacency list in a directed graph, each bucket has a linked list for outgoing edges and for incoming edges. To reverse it, simply switch the outgoing and incoming edges.
  * It's just pointer manipulation. Pretty fast :D

---

### Why Does it Work?

#### The SCC Graph

![image-20230510151119172](attachments/cs161.assets/image-20230510151119172.png)

#### Lemma 1

![image-20230510151052351](attachments/cs161.assets/image-20230510151052351.png)

#### Definitions

![image-20230510151151228](attachments/cs161.assets/image-20230510151151228.png)

#### Topological Sort

![image-20230510151244964](attachments/cs161.assets/image-20230510151244964.png)

* The topological sort of this SCC DAG would be from the Cal SCC, to the Stanford SCC, to the puppies SCC.

![image-20230510151434162](attachments/cs161.assets/image-20230510151434162.png)

* If I now run DFS in that SCC with the largest finish time, I'll just explore that specific SCC, since you can never get out of the SCC.
* Once we're done, we can remove that SCC and repeat.

### Formal Proof

![image-20230510151601751](attachments/cs161.assets/image-20230510151601751.png)

* This claim used the idea with the tree and the descendants.

![image-20230510151648770](attachments/cs161.assets/image-20230510151648770.png)

#### Lemma 2

![image-20230510151659339](attachments/cs161.assets/image-20230510151659339.png)

#### Proof of Lemma

![image-20230510151741818](attachments/cs161.assets/image-20230510151741818.png)

![image-20230510151901582](attachments/cs161.assets/image-20230510151901582.png)

![image-20230510151942301](attachments/cs161.assets/image-20230510151942301.png)

---

#### Corollary 1

![image-20230510152022893](attachments/cs161.assets/image-20230510152022893.png)

* This is true because the original graph would have an edge going from $A$ to $B$, which means the lemma holds.

---

#### Why This Finds SCCs

![image-20230510152118295](attachments/cs161.assets/image-20230510152118295.png)

---

### Proof by Induction

![image-20230510152207932](attachments/cs161.assets/image-20230510152207932.png)

* Once we run DFS after the first run $t$ times, then we've found the first $t$ SCCs.

![image-20230510152304228](attachments/cs161.assets/image-20230510152304228.png)

* If $A$ is strongly connected before, if we reverse all the edges, then $A$ is still going to be strongly connected.

---

### Recap of Proof

![image-20230510152404279](attachments/cs161.assets/image-20230510152404279.png)

![image-20230510152440095](attachments/cs161.assets/image-20230510152440095.png)

---

### Lesson Recap

![image-20230510152610098](attachments/cs161.assets/image-20230510152610098.png)

---

## 5/10 L11: Dijkstra's and Bellman-Ford

### Weighted Graphs

* BFS does not work very well with weighted graphs, since it just minimizes the number of links.

![image-20230515154113955](attachments/cs161.assets/image-20230515154113955.png)

---

### Shortest Path Problem

![image-20230515154201273](attachments/cs161.assets/image-20230515154201273.png)

---

### Sub-paths

* Recall that the below is the shortest path from Gates to Old Union.

![image-20230515154314478](attachments/cs161.assets/image-20230515154314478.png)

* It must be true that the shortest path from Package to Old Union is the same path, removing Gates.

![image-20230515154335442](attachments/cs161.assets/image-20230515154335442.png)

* We prove the claim above my contradiction.

---

### Single-source Shortest-path Problem

![image-20230515154504118](attachments/cs161.assets/image-20230515154504118.png)

* The table above is for the graph from the images before.

---

### Dijkstra's Algorithm

* Dijkstra’s algorithm finds shortest paths in weighted graphs with non-negative edge weights.

![image-20230515154827239](attachments/cs161.assets/image-20230515154827239.png)

* Think of the edge weights like the length of the string. The order that the things get pulled off the ground are its shortest paths.

---

### Dijkstra's Illustration

![image-20230515154946415](attachments/cs161.assets/image-20230515154946415.png)

* We pick `Gates` first because it is the `not-sure` node with the smallest estimate `d[u]` (it's estimate is zero because that's where we start)

![image-20230515155018583](attachments/cs161.assets/image-20230515155018583.png)

* All the neighbors get the minimum between what it current has, and what its weight would be if we were to connect `u` to that neighbor.
* Notice that the estimate for `Packard` and `Dish` are changed.

![image-20230515155315386](attachments/cs161.assets/image-20230515155315386.png)

* We now repeat with the next smallest estimate, which is `Packard`. In this case, it also updates `CS161` and `Dish` again, since the new value is less than before.
* We just repeat this process.

![image-20230515155414105](attachments/cs161.assets/image-20230515155414105.png)

---

### Dijkstra's Pseudocode

![image-20230515155440971](attachments/cs161.assets/image-20230515155440971.png)

---

### Why Does Dijkstra's Work?

![image-20230515155800464](attachments/cs161.assets/image-20230515155800464.png)

* What Claim 1 and Claim 2 are basically saying is that `d[v]` is an overestimate, and will gradually decreases until we get the right answer.
* `d[v]` never increases because the algorithm replaces it with the minimum of itself with something else. Can only decrease or stay same.

#### Claim 1: `d[v]` is Always an Overestimate

![image-20230515160136227](attachments/cs161.assets/image-20230515160136227.png)

![image-20230515160222098](attachments/cs161.assets/image-20230515160222098.png)

#### Claim 2: When Vertex `u` is Marked Sure, `d[u] = d(s, u)`

![image-20230515160328066](attachments/cs161.assets/image-20230515160328066.png)

![image-20230515160343848](attachments/cs161.assets/image-20230515160343848.png)

* If edge weights are negative, it's possible for there to be a path from `s` to itself with a negative weight.

![image-20230515160533610](attachments/cs161.assets/image-20230515160533610.png)

![image-20230515160558044](attachments/cs161.assets/image-20230515160558044.png)

* We now claim the following:

![image-20230515160834077](attachments/cs161.assets/image-20230515160834077.png)

* The first equality is true because we said that `z` is good.
* The second inequality is true because the path from s to z is a subpath from s to u.

![image-20230515160904369](attachments/cs161.assets/image-20230515160904369.png)

* The last inequality is just what we prove in Claim 1.

![image-20230515160946704](attachments/cs161.assets/image-20230515160946704.png)

* z must be marked as sure because we chose u so that `d[u]` was the smallest of the unsure vertices.
  * Since `d[z] < d[u]`, it must have already been marked sure, otherwise we would've just picked `z`

![image-20230515161039377](attachments/cs161.assets/image-20230515161039377.png)

* It's a contradiction because we said `z'` was not good.

---

### Running time of Dijkstra

![image-20230515201427204](attachments/cs161.assets/image-20230515201427204.png)

* The while loop has $n$ iterations (one per vertex)
* The runtime of each iteration is dependent on implementation
  * Requires a data structure with a few things

![image-20230515201537880](attachments/cs161.assets/image-20230515201537880.png)

* The outside sum is the sum over all vertices. Inside the sum is the content of each iteration.
  * At each, we need to find the minimum. Then, for every neighbor, we need to update the key. Then, we need to remove the minimum.
  * The equal sign is Big-O of blah blah blah

* `removeMin` is from when we mark `u` as sure. We need to remove it from the data structure so that we don't consider it again.

#### Array Implementation

![image-20230515201949509](attachments/cs161.assets/image-20230515201949509.png)

#### Red-black Tree

![image-20230515202058202](attachments/cs161.assets/image-20230515202058202.png)

* This is better than an array if $m$ is closer to 1 than $n$, then it is $n\log n$.
* If $m$ is like $n^2$, then this is like $n^2 \log n$

#### Fibonacci Heap

![image-20230515202228475](attachments/cs161.assets/image-20230515202228475.png)

---

#### Runtime in Practice

![image-20230515202350777](attachments/cs161.assets/image-20230515202350777.png)

---

### Drawbacks of Dijkstra's

![image-20230515202457000](attachments/cs161.assets/image-20230515202457000.png)

---

### Bellman-Ford Algorithm

![image-20230515202658227](attachments/cs161.assets/image-20230515202658227.png)

![image-20230515202721853](attachments/cs161.assets/image-20230515202721853.png)

---

#### Definition by Example

* First we initialize $n$ arrays.

![image-20230515202836290](attachments/cs161.assets/image-20230515202836290.png)

* Instead of updating just one thing, we update all of the neighbors.

![image-20230515202859161](attachments/cs161.assets/image-20230515202859161.png)

![image-20230515202937782](attachments/cs161.assets/image-20230515202937782.png)

![image-20230515202948916](attachments/cs161.assets/image-20230515202948916.png)

![image-20230515202958224](attachments/cs161.assets/image-20230515202958224.png)

![image-20230515203006067](attachments/cs161.assets/image-20230515203006067.png)

* We basically go through every node, and then go through all its neighbors and asks if they have a shorter path.

---

### Does it Work and is it Fast?

![image-20230515203403072](attachments/cs161.assets/image-20230515203403072.png)

![image-20230515203511629](attachments/cs161.assets/image-20230515203511629.png)

---

### Negative Cycles

![image-20230519120319478](attachments/cs161.assets/image-20230519120319478.png)

![image-20230515203542524](attachments/cs161.assets/image-20230515203542524.png)

![image-20230515203555747](attachments/cs161.assets/image-20230515203555747.png)

---

### Bellman-Ford Summary

![image-20230515203611787](attachments/cs161.assets/image-20230515203611787.png)

---

### Shortest Paths Recap

![image-20230515203632489](attachments/cs161.assets/image-20230515203632489.png)

---

## 5/15 L12: DF, FW, DP

![image-20230519120225872](attachments/cs161.assets/image-20230519120225872.png)

### Single-source Shortest Path

* Dijkstra and Bellman-Ford both solve this problem

![image-20230519120251582](attachments/cs161.assets/image-20230519120251582.png)

### Bellman-Ford

![image-20230519120746732](attachments/cs161.assets/image-20230519120746732.png)

![image-20230519120829085](attachments/cs161.assets/image-20230519120829085.png)

#### Interpretation of $d^{(i)}$

![image-20230519120907350](attachments/cs161.assets/image-20230519120907350.png)

* If we look at Old Union
  * For d0, the shortest path between Gates and Old Union with zero edges is infinity
  * For d1, there is still no paths with one edges, so its still infinity.
  * For d2, there is a path with two edges, from gates to dish to old union
  * For d3, there exists a path with three edges, from Gates to Packard to CS161 to Old Union.
  * For d4, this is the same as d3.

#### Correctness Proof with DP

![image-20230519121314714](attachments/cs161.assets/image-20230519121314714.png)

* With no negative cycles, cycles either just add more or sum to zero. As such, we can assume there are no cycles.

![image-20230519121436040](attachments/cs161.assets/image-20230519121436040.png)

#### Most Important Thing Abt BF

![image-20230519122548676](attachments/cs161.assets/image-20230519122548676.png)

### Fibonacci

![image-20230519122704818](attachments/cs161.assets/image-20230519122704818.png)

* As seen in the graph above, this grows very very fast.

![image-20230519122728976](attachments/cs161.assets/image-20230519122728976.png)

![image-20230519124521872](attachments/cs161.assets/image-20230519124521872.png)

![image-20230519124529026](attachments/cs161.assets/image-20230519124529026.png)

* The naive algorithm is exponential, and the faster one is linear.

### Dynamic Programming

![image-20230519124931584](attachments/cs161.assets/image-20230519124931584.png)

#### Optimal Sub-structure

![image-20230519125051285](attachments/cs161.assets/image-20230519125051285.png)

* This is the same as in divide and conquer, where we use the smaller problems to approach and solve our bigger problem.

#### Overlapping Sub-problems

![image-20230519125208856](attachments/cs161.assets/image-20230519125208856.png)

#### Elements of DP

![image-20230519125250789](attachments/cs161.assets/image-20230519125250789.png)

### Bottom up Approach to DP

* In the bottom up approach, you solve smaller problems to build up to the bigger problem.



![image-20230519125329325](attachments/cs161.assets/image-20230519125329325.png)

![image-20230519125337747](attachments/cs161.assets/image-20230519125337747.png)

### Top down Approach to DP

* The top-down approach is defined by the idea of memo-ization. You memoize the things that you have solved previously.

![image-20230519125357605](attachments/cs161.assets/image-20230519125357605.png)

![image-20230519125435566](attachments/cs161.assets/image-20230519125435566.png)

* Top down is just recursive, but you remember the subproblems.
* For every solution that uses top-down, there is also a bottom-up version that fills the solution in the exact same order.
  * You take the top down solution, see where it fills up the solution, then write a bottom up solution that fills it in that order.

---

### Memo-ization

![image-20230522124757752](attachments/cs161.assets/image-20230522124757752.png)

![image-20230522124803178](attachments/cs161.assets/image-20230522124803178.png)

---

### Dynamic Programming Recap

![image-20230522125005992](attachments/cs161.assets/image-20230522125005992.png)

### Recipe for DP

![image-20230522125506873](attachments/cs161.assets/image-20230522125506873.png)

---

### Floyd-Warshall Algorithm

![image-20230522125237178](attachments/cs161.assets/image-20230522125237178.png)

---

#### Naive Solution

![image-20230522125329135](attachments/cs161.assets/image-20230522125329135.png)

---

### Step 1: Optimal Substructure

![image-20230522125643804](attachments/cs161.assets/image-20230522125643804.png)

![image-20230522125608382](attachments/cs161.assets/image-20230522125608382.png)

* The shortest path from $u$ to $v$ where the only vertices we're allowed to use is the ones in $1 \dots k-1$.
* There's $n$ vertices total.

### Step 2: Recursive Formulation

![image-20230522125827435](attachments/cs161.assets/image-20230522125827435.png)

* There are two cases for this. Either the shortest path from $u$ to $v$ through the dark blue blob involves $k$, or it does not involve $k$.

![image-20230522125911398](attachments/cs161.assets/image-20230522125911398.png)

![image-20230522125936270](attachments/cs161.assets/image-20230522125936270.png)

* This works because in the $k-1$ level, we've already computed the shortest path for all pairs, not just from $u$ to $v$.

#### Recap

![image-20230522130126469](attachments/cs161.assets/image-20230522130126469.png)

![image-20230522130138402](attachments/cs161.assets/image-20230522130138402.png)

### Step 3: Use DP

![image-20230522130433880](attachments/cs161.assets/image-20230522130433880.png)

### FW Theorem

![image-20230522130659778](attachments/cs161.assets/image-20230522130659778.png)

#### Negative Cycles

![image-20230522130807039](attachments/cs161.assets/image-20230522130807039.png)

---

## 5/17 L13: LCS, Knapsack

### DP Recap

![image-20230522144417340](attachments/cs161.assets/image-20230522144417340.png)

* Bottom up is starting explicitly with the smaller problems and filling up.
* Top down is doing like recursion but with memoization

---

### Longest Common Subsequence

![image-20230522144657431](attachments/cs161.assets/image-20230522144657431.png)

#### Problem Definition

![image-20230522144737481](attachments/cs161.assets/image-20230522144737481.png)

#### Recipe Steps

![image-20230522144940614](attachments/cs161.assets/image-20230522144940614.png)

### Step 1: Optimal Substructure

* We're trying to find subproblems in a sequence. The natural thing is to look at prefixes of the string.

![image-20230522145036319](attachments/cs161.assets/image-20230522145036319.png)

![image-20230522145117807](attachments/cs161.assets/image-20230522145117807.png)

* `C[2,3]` is the LCS between `AC` and `ACG`, which is just `AC`.
* `C[4,4]` is the LCS between `ACGG` and `ACGC`, which is just `ACG`.

* We can build up our answer by finding the LCS's of prefixes. Once we reach the end, we have the LCS of the entire thing.

### Step 2: Recursive Formulation

![image-20230522145235914](attachments/cs161.assets/image-20230522145235914.png)

* This breaks down into cases. Either the last symbol is the same, or they are different.
  * We care about the last because we essentially have the prefixes, and we're concerned with adding letters one at a time until we reach the whole thing.
  * Basically what happens when we add this last letter.

#### Case 1

* If `X[i] = Y[j]`, we just find the LCS of `X_{i-1}` and `Y_{i-1}`, and then add one.

![image-20230522145454699](attachments/cs161.assets/image-20230522145454699.png)

#### Case 2

* If `X[i] != Y[j]`, we can take the maximum value of `i-1, j` and `j, i-1`.
  * Either the LCS between X_i and Y_i is just between the first four symbols of X and the entire Y prefix, and T is not involved.
  * Or the first six symbols of Y, and the entire X prefix, and A is not involved.

![image-20230522145647903](attachments/cs161.assets/image-20230522145647903.png)

#### Formulation

![image-20230522145840788](attachments/cs161.assets/image-20230522145840788.png)

### Step 3: Pseudocode

![image-20230522145855963](attachments/cs161.assets/image-20230522145855963.png)

* The running time of this algorithm is $O(nm)$. We walk through the entire table and fill it up.
* $C$ is an `mxn` matrix.

### Running Example

![image-20230522150038949](attachments/cs161.assets/image-20230522150038949.png)

* The zeros are our base case.

![image-20230522150107395](attachments/cs161.assets/image-20230522150107395.png)

* The two A's match up, so that's why its a `1`.

![image-20230522150132793](attachments/cs161.assets/image-20230522150132793.png)

* Now, we see that A is not equal to C. Therefore, we find the maximum of the one that is right above the cell, or the one directly to the left of it.

![image-20230522150232268](attachments/cs161.assets/image-20230522150232268.png)

* After fully running it, we get this table.
* In the case that Y is all A's, that second row would still be the same. It would see the A in X match with all the A's in Y, which would go up a diagonal and add one to the zero in the first row.

### Step 4: Recovering the LCS

* We can do this by looking at the table itself.

![image-20230522150541110](attachments/cs161.assets/image-20230522150541110.png)

* We look at the resulting box, and see where it comes from.
  * We see that A and G are not the same, so we must've been in case 2. Therefore, it must've either came from the box above or the one to the left.
  * By inspection, it came from the box above.

![image-20230522150842694](attachments/cs161.assets/image-20230522150842694.png)

![image-20230522150855864](attachments/cs161.assets/image-20230522150855864.png)

* Now we see that it doesn't match, so we're in case 2 again. It could come from either of the boxes.

![image-20230522150924089](attachments/cs161.assets/image-20230522150924089.png)

![image-20230522150939294](attachments/cs161.assets/image-20230522150939294.png)

![image-20230522150947754](attachments/cs161.assets/image-20230522150947754.png)

#### Recap

![image-20230522151053485](attachments/cs161.assets/image-20230522151053485.png)

### Step 5: Code

![image-20230522151123484](attachments/cs161.assets/image-20230522151123484.png)

![image-20230522151314905](attachments/cs161.assets/image-20230522151314905.png)

---

### Knapsack Problem

![image-20230522151354440](attachments/cs161.assets/image-20230522151354440.png)

* There are two different formulations of the Knapsack problem

![image-20230522151505650](attachments/cs161.assets/image-20230522151505650.png)

### Unbounded Knapsack

#### Notation

![image-20230522151538309](attachments/cs161.assets/image-20230522151538309.png)

#### Optimal Substructure

* We can think of it like building up from smaller knapsacks.

![image-20230522151618829](attachments/cs161.assets/image-20230522151618829.png)

* Consider an arbitrary item i, and suppose that the optimal solution for capacity `x` involves item `i`.

![image-20230522151811809](attachments/cs161.assets/image-20230522151811809.png)

* This is the same logic as the shortest paths and subpaths of the shortest paths.

#### Recursive Formulation

![image-20230522194918709](attachments/cs161.assets/image-20230522194918709.png)

#### Dynamic Programming

![image-20230522195318299](attachments/cs161.assets/image-20230522195318299.png)

* The `K[x] = 0` is basically the base case. If none of the $w_i$ are less than $x$, then the best value we can have with capacity x is zero.

#### Finding Actual Solution

* What if we want to find the actual items?

![image-20230522195426953](attachments/cs161.assets/image-20230522195426953.png)

* The intuition is that whenever $K[x]$ is updated, that means adding $w_i$ was a good thing to do, so we should maybe consider using that item.

#### Example

##### X = 0 and X = 1

![image-20230522195546616](attachments/cs161.assets/image-20230522195546616.png)

##### X = 2

* At this point, we could add a turtle.

![image-20230522195601289](attachments/cs161.assets/image-20230522195601289.png)

* We could also add a light bulb. the lightbulb gives us a better value, so we choose that.

![image-20230522195616838](attachments/cs161.assets/image-20230522195616838.png)

##### k=3

* I could add a turtle to the light bulb

![image-20230522195712301](attachments/cs161.assets/image-20230522195712301.png)

* I could also add a watermelon. This has better value.

![image-20230522195718293](attachments/cs161.assets/image-20230522195718293.png)

##### K = 4

![image-20230522195750808](attachments/cs161.assets/image-20230522195750808.png)

![image-20230522195758601](attachments/cs161.assets/image-20230522195758601.png)

* Note that I can also add a watermelon to `K[1]`. However, that gives me a worse value than what I have now, so I don't use that.

#### Recap

![image-20230522195903275](attachments/cs161.assets/image-20230522195903275.png)

---

### 0/1 Knapsack

![image-20230529170357308](attachments/cs161.assets/image-20230529170357308.png)

#### Optimal Substructure

* The subproblem of unbounded knapsack isn't gonna work here

![image-20230529170522779](attachments/cs161.assets/image-20230529170522779.png)

* This means we now have to look at the knapsack, but also what items we're allowed to use for the smaller kanpsack

![image-20230529170553596](attachments/cs161.assets/image-20230529170553596.png)

![image-20230529170610292](attachments/cs161.assets/image-20230529170610292.png)

![image-20230529170621093](attachments/cs161.assets/image-20230529170621093.png)

#### Recursive Relationship

* Consider item $j$, the last item that we're adding. In the pictures, item $j$ is the turtle.

![image-20230529170734601](attachments/cs161.assets/image-20230529170734601.png)

* In this case, `K[x, j] = K[x, j-1]`

![image-20230529170845995](attachments/cs161.assets/image-20230529170845995.png)

* In this case, `K[x, j] = K[x - w_j, j - 1] + v_j`
* We use the smaller knapsack here because there's a turtle in the smaller optimal solution, so we take the turtle out

![image-20230529171053703](attachments/cs161.assets/image-20230529171053703.png)

* The base cases say if $j = 0$, so there are no items, then you can have no value. If $x = 0$, so you have no capacity, then you also have no value.

#### Bottom-up DP Algorithm

![image-20230529171158777](attachments/cs161.assets/image-20230529171158777.png)

* The running time of this is $O(nW)$

#### Example

![image-20230529171451470](attachments/cs161.assets/image-20230529171451470.png)

#### Summary

![image-20230529171525772](attachments/cs161.assets/image-20230529171525772.png)

---

## 5/22 L14: Greedy Algorithm

---

### Top-Down Vs Bottom-up DP

* Top down is the same as divide and conquer, except you keep a table so you don't repeat work.
* In bottom up, you fill that table iteratively from the bottom up.

![image-20230529172201449](attachments/cs161.assets/image-20230529172201449.png)

### What Are Greedy Algorithms?

![image-20230529172304808](attachments/cs161.assets/image-20230529172304808.png)

### When Does Greedy not Work?

* An example of when it doesn't work is unbounded knapsack

![image-20230529172428751](attachments/cs161.assets/image-20230529172428751.png)

* In this example, the greedy part is picking the item with the best value/weight ratio.
* There is a natural greedy algorithm to this, but it doesn't work since it's possible to mix items to get closer to the weight limit with a higher value.

---

### Problem 1: Activity Selection

* This is an example where greedy algorithm works

![image-20230529172608668](attachments/cs161.assets/image-20230529172608668.png)

![image-20230529172626520](attachments/cs161.assets/image-20230529172626520.png)

#### Approaches

* We investigate each of the greedy ways to pick an activity.

![image-20230529173305165](attachments/cs161.assets/image-20230529173305165.png)

* This wouldn't work. If we pick the shortest job, we only have 1 activity. If we picked the other longer jobs, we have 2 activities.

![image-20230529173343276](attachments/cs161.assets/image-20230529173343276.png)

* Bruh moment

![image-20230529173351601](attachments/cs161.assets/image-20230529173351601.png)

### Greedy Algorithm for Activity Selection

![image-20230529173428925](attachments/cs161.assets/image-20230529173428925.png)

* The yellow is the ones we picked. The gray are the ones that are ruled out because we picked an overlap.
* Taking the smallest finishing time and latest start time would both give optimal solutions.
  * Think of it as if you just reversed your activities.

#### Is it Fast?

![image-20230529173603420](attachments/cs161.assets/image-20230529173603420.png)

#### Why is it Greedy?

![image-20230529173636009](attachments/cs161.assets/image-20230529173636009.png)

![image-20230529173741759](attachments/cs161.assets/image-20230529173741759.png)

#### Does it Work?

![image-20230529173812996](attachments/cs161.assets/image-20230529173812996.png)

* If we first pick $a_1$, our next choice would be $a_5$.
* Claim: For every choice we make, there still exists some optimal solution that extends it.

![image-20230529173917043](attachments/cs161.assets/image-20230529173917043.png)

#### Proof of the Claim

![image-20230529173928137](attachments/cs161.assets/image-20230529173928137.png)

* The optimal solution is $T^*$

![image-20230529173945673](attachments/cs161.assets/image-20230529173945673.png)

* If our next choice is in the optimal solution, then we're goodie

![image-20230529174043479](attachments/cs161.assets/image-20230529174043479.png)

* $a_j$ is the activity in the remaining part of the optimal solution with the smallest ending time.

![image-20230529174049333](attachments/cs161.assets/image-20230529174049333.png)

* If it's not, consider swapping what we picked with what's in the optimal solution.

![image-20230529174258029](attachments/cs161.assets/image-20230529174258029.png)

![image-20230529174310297](attachments/cs161.assets/image-20230529174310297.png)

#### Proof of Correctness

![image-20230531013234820](attachments/cs161.assets/image-20230531013234820.png)

* In the above, the current solution refers to the output at the end of the algorithm.

---

### Strategy 1 for Greedy Algorithms

![image-20230531013653634](attachments/cs161.assets/image-20230531013653634.png)

![image-20230531013713076](attachments/cs161.assets/image-20230531013713076.png)

![image-20230531013743400](attachments/cs161.assets/image-20230531013743400.png)

---

### When Are Greedy Algorithms a Good Idea?

* When they have a particular type of substructure

![image-20230531013932710](attachments/cs161.assets/image-20230531013932710.png)

* We represent each in the form of a tree.

![image-20230531013953609](attachments/cs161.assets/image-20230531013953609.png)

* For divide and conquer, it was just a decision tree.

![image-20230531014008342](attachments/cs161.assets/image-20230531014008342.png)

* DP was kind of like a DAG, where the same subproblem was used by many other bigger subproblems.

![image-20230531014040866](attachments/cs161.assets/image-20230531014040866.png)

* Greedy is like a linear tree. You make one big greedy choice, then solve the smaller problem from that.

---

### Problem 2: Scheduling

![image-20230531014242782](attachments/cs161.assets/image-20230531014242782.png)

* One person may have a lot of activities that they want to do that takes up a certain amount of time.

![image-20230531014551035](attachments/cs161.assets/image-20230531014551035.png)

* Essentially the costs are added up with every activity before it, until it is completed.

![image-20230531014627412](attachments/cs161.assets/image-20230531014627412.png)

#### Optimal Substructure

![image-20230531014649614](attachments/cs161.assets/image-20230531014649614.png)

* This is true because of the same argument we used before where the subpaths of shortest paths are also shortest paths.
* If it was not the optimal schedule on just jobs B, C, D, then you can rearrange it to make a better schedule for A, B, C, D

![image-20230531014820573](attachments/cs161.assets/image-20230531014820573.png)

* Optimal solutions to subproblems tell us something about optimal solutions to bigger problems.

![image-20230531015026550](attachments/cs161.assets/image-20230531015026550.png)

#### Best Job

* What does it mean by taking the best job?

![image-20230531015229736](attachments/cs161.assets/image-20230531015229736.png)

![image-20230531015247177](attachments/cs161.assets/image-20230531015247177.png)

#### Proving that the Greedy Choice Doesn't Rule out Success

![image-20230531015600621](attachments/cs161.assets/image-20230531015600621.png)

* This is optimal because at each swap, we haven't increased the overall cost of the schedule.
* This is because we said $B$ had the best ratio of delay/time, which basically means B then A is better than A then B.
  * We keep repeating these swaps until $B$ is at the front without ever raising the cost.

#### Proof

![image-20230531015813665](attachments/cs161.assets/image-20230531015813665.png)

* The inductive hypothesis is: after we've chosen the $t$'th thing in our schedule, there still exists a optimal way of scheduling things that extends our choices.

#### Algorithm

![image-20230531015943447](attachments/cs161.assets/image-20230531015943447.png)

#### Summary

![image-20230531020019829](attachments/cs161.assets/image-20230531020019829.png)

* This works here but doesn't work in knapsack because of the difference in the optimal substructure.
  * Here, the optimal substructure is basically just a linear tree. For knapsack, that one was more a DAG with branching like with DP
  * In Knapsack, we had the constraint of the weight and also the fact that you can't put fractional portions of items.
    * If you could put like half a taco, then the greedy algorithm would be the optimal algorithm.

---

### Problem 3: Huffman Coding

* Coding refers to encoding data

![image-20230531020328083](attachments/cs161.assets/image-20230531020328083.png)

* What is the best way to store this string data?

![image-20230531020352316](attachments/cs161.assets/image-20230531020352316.png)

![image-20230531020412328](attachments/cs161.assets/image-20230531020412328.png)

![image-20230531020454702](attachments/cs161.assets/image-20230531020454702.png)

![image-20230531020500008](attachments/cs161.assets/image-20230531020500008.png)

![image-20230531020531199](attachments/cs161.assets/image-20230531020531199.png)

* What is the most efficient way to do prefix-free coding?

![image-20230531020556739](attachments/cs161.assets/image-20230531020556739.png)

* We want more frequent strings to be higher up, and the less frequent strings to be lower down.

![image-20230531020643760](attachments/cs161.assets/image-20230531020643760.png)

* We make an interior node that sums the frequencies. Then, we iterate until we no longer have any more.

![image-20230531020722617](attachments/cs161.assets/image-20230531020722617.png)

![image-20230531020728830](attachments/cs161.assets/image-20230531020728830.png)

---

### Recap

![image-20230531020821528](attachments/cs161.assets/image-20230531020821528.png)

---

## 5/24 L15: Minimum Spanning Trees

### Greedy Recap

![image-20230604122727410](attachments/cs161.assets/image-20230604122727410.png)

---

### What is a MST?

![image-20230604122808870](attachments/cs161.assets/image-20230604122808870.png)

* Note that we only consider connected graphs for now.
* Spanning trees are not unique.

![image-20230604122906080](attachments/cs161.assets/image-20230604122906080.png)

![image-20230604122923764](attachments/cs161.assets/image-20230604122923764.png)

![image-20230604122931293](attachments/cs161.assets/image-20230604122931293.png)

---

### How to Find an MST?

![image-20230604123207884](attachments/cs161.assets/image-20230604123207884.png)

---

### Cuts in Graphs

![image-20230604123734151](attachments/cs161.assets/image-20230604123734151.png)

* Note that cuts don't necessarily have to be contiguous.

![image-20230604123814510](attachments/cs161.assets/image-20230604123814510.png)

![image-20230604125014627](attachments/cs161.assets/image-20230604125014627.png)

![image-20230604125127933](attachments/cs161.assets/image-20230604125127933.png)

* Respects means that no edges in $S$ cross the cut. It does not mean that every edge that doesn't cross the cut is in $S$.

![image-20230604125239139](attachments/cs161.assets/image-20230604125239139.png)

### Lemma with Cuts

![image-20230604125352486](attachments/cs161.assets/image-20230604125352486.png)

* By "safe" to add this edge, this means that choosing that edge would not rule out the existence of an MST.
* Also, this says pick an arbitrary light edge. It does not say consider all light edges.

![image-20230604125403083](attachments/cs161.assets/image-20230604125403083.png)

#### Proof of Lemma

![image-20230604125932966](attachments/cs161.assets/image-20230604125932966.png)

![image-20230604125940336](attachments/cs161.assets/image-20230604125940336.png)

![image-20230604130154256](attachments/cs161.assets/image-20230604130154256.png)

* It makes a cycle because we know that $u$ and $v$ are already connected to each other through the MST $T$. As such, adding another edge to connect it must make a cycle.
  * This doesn't apply to just light edges. Adding any edge to an MST will create a cycle.

![image-20230604130323023](attachments/cs161.assets/image-20230604130323023.png)

![image-20230604130756072](attachments/cs161.assets/image-20230604130756072.png)

---

### MSTs

![image-20230604131157395](attachments/cs161.assets/image-20230604131157395.png)

---

### Prim's Algorithm

![image-20230604134316338](attachments/cs161.assets/image-20230604134316338.png)

![image-20230604134333276](attachments/cs161.assets/image-20230604134333276.png)

![image-20230604134349403](attachments/cs161.assets/image-20230604134349403.png)

* Make sure you don't add any edges that can cause a cycle.

#### Pseudocode

![image-20230604134548295](attachments/cs161.assets/image-20230604134548295.png)

![image-20230604134613756](attachments/cs161.assets/image-20230604134613756.png)

#### Does it Work?

![image-20230604134727972](attachments/cs161.assets/image-20230604134727972.png)

![image-20230604135219657](attachments/cs161.assets/image-20230604135219657.png)

![image-20230604135255065](attachments/cs161.assets/image-20230604135255065.png)

![image-20230604135318871](attachments/cs161.assets/image-20230604135318871.png)

#### Implementation

![image-20230604135440566](attachments/cs161.assets/image-20230604135440566.png)

![image-20230604135514256](attachments/cs161.assets/image-20230604135514256.png)

![image-20230604135523956](attachments/cs161.assets/image-20230604135523956.png)

![image-20230604135631688](attachments/cs161.assets/image-20230604135631688.png)

![image-20230604135715339](attachments/cs161.assets/image-20230604135715339.png)

![image-20230604135800816](attachments/cs161.assets/image-20230604135800816.png)

#### Runtime

![image-20230604135823647](attachments/cs161.assets/image-20230604135823647.png)

---

### Kruskal's Algorithm

![image-20230604140038559](attachments/cs161.assets/image-20230604140038559.png)

![image-20230604140046759](attachments/cs161.assets/image-20230604140046759.png)

![image-20230604140111203](attachments/cs161.assets/image-20230604140111203.png)

![image-20230604140117969](attachments/cs161.assets/image-20230604140117969.png)

![image-20230604140131830](attachments/cs161.assets/image-20230604140131830.png)

![image-20230604140147793](attachments/cs161.assets/image-20230604140147793.png)

#### Pseudocode

![image-20230604140247612](attachments/cs161.assets/image-20230604140247612.png)

* One way to check if it doesn't cause a cycle is similar to Prim's, where we check if one end vertex is visited and the other end is unvisited.
* The final tree will be connected because if we had two unconnected trees, there must exist an edge that we can add that wouldn't make a cycle.
  * We only stop once we have $n$ vertices with $n-1$ edges and we have no cycles.

#### How Do We Make a Fast Implementation?

![image-20230604140601968](attachments/cs161.assets/image-20230604140601968.png)

![image-20230604140608899](attachments/cs161.assets/image-20230604140608899.png)

* Notice that all these trees are disjoint. As such, we can keep each tree in a special data structure.

#### Union-find Data Structure

![image-20230604140710415](attachments/cs161.assets/image-20230604140710415.png)

#### Fast Pseudocode

![image-20230604140809459](attachments/cs161.assets/image-20230604140809459.png)

![image-20230604140738251](attachments/cs161.assets/image-20230604140738251.png)

* We still once we have one big tree!

#### Running Time

![image-20230604140927937](attachments/cs161.assets/image-20230604140927937.png)

* Sorting the edges technically takes $O(m \log m)$. However, in a connected graph, $n < m < n^2$, so $\log m$ and $\log n$ are the same big O.

![image-20230604141058325](attachments/cs161.assets/image-20230604141058325.png)

#### Does it Work?

![image-20230604141343530](attachments/cs161.assets/image-20230604141343530.png)

![image-20230604141359575](attachments/cs161.assets/image-20230604141359575.png)

![image-20230604141423563](attachments/cs161.assets/image-20230604141423563.png)



### Recap of MST Algorithms

![image-20230604141245412](attachments/cs161.assets/image-20230604141245412.png)

![image-20230604141308968](attachments/cs161.assets/image-20230604141308968.png)

---

## 5/31 L16: Max-flows, Min-cuts, Ford-Fulkerson

### Graph Setup

![image-20230605145103090](attachments/cs161.assets/image-20230605145103090.png)

* Capacities are the same as weights. We're using a different name for it here.

### S-t Cut

* A cut that separates the source vertex $s$ from the sink vertex $t$.

![image-20230605145142042](attachments/cs161.assets/image-20230605145142042.png)

![image-20230605145258976](attachments/cs161.assets/image-20230605145258976.png)

* The cut above has cost $4 + 2 + 10 = 16$

### Minimum S-t Cut

![image-20230605145324426](attachments/cs161.assets/image-20230605145324426.png)

* Only count edges that go from $s$'s side of the cut to $t$'s side of the cut.

#### Why is Min S-t Cut Important?

![image-20230605145504245](attachments/cs161.assets/image-20230605145504245.png)

### Flows

* Think of capacity as how much an edge can hold, and flow has how much can cross a certain edge.

![image-20230605145719459](attachments/cs161.assets/image-20230605145719459.png)

* The edge with the red mark can hold 4 stuff, and there's 2 units of stuff going across it.
* The flow must be less than or equal to the capacity.
* Except for the source and the sink, the incoming flows must equal outgoing flows.

#### Value of a Flow

![image-20230605145811932](attachments/cs161.assets/image-20230605145811932.png)

* It's okay to have cycles and flow cycles in a graph, as long as it still follows the conservation of flow at vertices.

### Max Flow

![image-20230605145956983](attachments/cs161.assets/image-20230605145956983.png)

![image-20230605150003834](attachments/cs161.assets/image-20230605150003834.png)

* Note that in this example, 11 was also the cost of the minimum cut.

#### Why Do We Care about Max Flow?

![image-20230605150100267](attachments/cs161.assets/image-20230605150100267.png)

### Min Cut and Max Flow

![image-20230605150217944](attachments/cs161.assets/image-20230605150217944.png)

* The purple and red are the two flows of people.

![image-20230605150248763](attachments/cs161.assets/image-20230605150248763.png)

* The three colors are each stream of flow.
* Note that these flows can share vertices but not edges.

### Theorem

![image-20230605150405539](attachments/cs161.assets/image-20230605150405539.png)

* The orange edges are the ones cut in the min cut. Notice that in this max flow, those edges are full. Their capacity and their flow are equal.

### Corollary

![image-20230605150540164](attachments/cs161.assets/image-20230605150540164.png)

* If you can find a cut with the same cost as some flow, then you're guaranteed that those are the max flow and the min cut.

### Ford-Fulkerson Algorithm

![image-20230605150732487](attachments/cs161.assets/image-20230605150732487.png)

* We start with no flow, and maintain a residual graph.

#### Residual Graphs and Networks

![image-20230605150811339](attachments/cs161.assets/image-20230605150811339.png)

* The vertices are the exact same.
* However, we now have two types of edges.
  * The forward edges are in blue, going in the same direction as the edges in the original graph.
  * The backwards edges are in green, going in the opposite direction.

![image-20230605150909508](attachments/cs161.assets/image-20230605150909508.png)

![image-20230605150929559](attachments/cs161.assets/image-20230605150929559.png)

* Forward edges are the amount of capacity we have left to stuff stuff through, and the backwards edges are the amount of capacity that's already been used.

#### Improving Flow with Residual Networks

![image-20230605151025070](attachments/cs161.assets/image-20230605151025070.png)

* There is a way to mess with the flows on the yellow path to increase the total flow through the graph.

##### Case 1

![image-20230605151359762](attachments/cs161.assets/image-20230605151359762.png)

* Take the minimum weight on all the forward edges in the residual network, and increase all the flows on that path by that value.

![image-20230605151520106](attachments/cs161.assets/image-20230605151520106.png)

* Make sure to also update the residual graph. This would kill that augmenting path that we just used.

![image-20230605151541246](attachments/cs161.assets/image-20230605151541246.png)

##### Case 2

![image-20230605151715696](attachments/cs161.assets/image-20230605151715696.png)

* In this case, we have to increase and decrease the flow of different edges in our path to make it work. More specifically, we increase flow along forwards edges and decrease flow along backwards edges.

![image-20230605151949894](attachments/cs161.assets/image-20230605151949894.png)

![image-20230605152005177](attachments/cs161.assets/image-20230605152005177.png)

![image-20230605152018524](attachments/cs161.assets/image-20230605152018524.png)

* Note that the augmenting path is now gone from the residual graph.

![image-20230605152114462](attachments/cs161.assets/image-20230605152114462.png)

##### Proof

![image-20230605152203542](attachments/cs161.assets/image-20230605152203542.png)

* We have our flow path $G$ in the top, and the residual path below.

 ![image-20230605152227225](attachments/cs161.assets/image-20230605152227225.png)

* Now we find the minimum weight on the path in the residual, and update everything accordingly.

![image-20230605152259109](attachments/cs161.assets/image-20230605152259109.png)

* Then we add that value to all the forward edges, and subtract it from all the backwards edges.
* For the residual graph, we subtract that amount from all the edges in the path.
  * In particular, the edge where we got the value from is going to disappear, breaking the augmenting path.

* This algorithm allows us to increase the flow as long as we can find an augmenting path.

#### Pseudocode

![image-20230605153044937](attachments/cs161.assets/image-20230605153044937.png)

#### Example

* The original graph is on the top, and the residual graph is on the bottom.![image-20230605153108096](attachments/cs161.assets/image-20230605153108096.png)

* Now we find an $s-t$ path in the residual graph. There are a lot of paths, below is one.![image-20230605153154419](attachments/cs161.assets/image-20230605153154419.png)
  * Then, we increase the flow according to the specifications before. This case is the easy case. We find the smallest edge on the path and update the flows.

* We repeat with another easy path. This is the last easy path.

  ![image-20230605153329880](attachments/cs161.assets/image-20230605153329880.png)

* Now consider this path. This is a hard path since there is a backwards edge.

  ![image-20230605153506740](attachments/cs161.assets/image-20230605153506740.png)

  * Now we do the same thing. `2` is the smallest weight on that path, so we add that to all the forward edges and subtract it from all the backwards edges (in the original graph).

  ![image-20230605153539853](attachments/cs161.assets/image-20230605153539853.png)

* We keep going until there are no paths left.

![image-20230605153654486](attachments/cs161.assets/image-20230605153654486.png)

* There are no forward edges crossing the cut
* Thus, this is a max flow because the flow value of the orange is the same as the cost of the red cut.

* Additionally, in this case, there does not exist a cut that isn't a min-cut.

---

* In the red cut above, one side is all the vertices that are reachable from $s$ in the residual graph. On the other side is all the vertices that are not reachable from $s$.
  * By definition, this means there are no edges going across the cut because if there were, the other side would be reachable from $s$.
* When you make that particular cut, where you split the things reachable from $s$ and the things not reachable, that corresponds to the minimum cut and the maximum flow in the original graph.

---

#### Recap

* The Ford-Fulkerson algorithm repeatedly refines augmenting paths until it can't anymore and updates the flows along the paths.
* Once it finished, you have a maximum flow and a minimum cut.
* The minimum cut is the cut between everything that is reachable from $s$ in the residual graph and everything that is not reachable from $s$.

![image-20230605154540473](attachments/cs161.assets/image-20230605154540473.png)

---

### Runtime of Ford-Fulkerson

![image-20230605154558114](attachments/cs161.assets/image-20230605154558114.png)

![image-20230605154646049](attachments/cs161.assets/image-20230605154646049.png)

![image-20230605154652873](attachments/cs161.assets/image-20230605154652873.png)

![image-20230605154700833](attachments/cs161.assets/image-20230605154700833.png)

![image-20230605154708022](attachments/cs161.assets/image-20230605154708022.png)

![image-20230605154720527](attachments/cs161.assets/image-20230605154720527.png)

* We'll eventually get the right answer, but it will take $C$ steps instead of just 2 steps.

### Edmonds-Karp Algorithm

* This is just Ford-Fulkerson using BFS to pick the augmenting paths.

![image-20230605154832520](attachments/cs161.assets/image-20230605154832520.png)

### Recap of Ford-Fulkerson

![image-20230605154845650](attachments/cs161.assets/image-20230605154845650.png)

### Aside about Max Flows

![image-20230605154929609](attachments/cs161.assets/image-20230605154929609.png)

---

### Application: Maximum Bipartite Matching

* Remember that a bipartite graph was one where you have vertices on one side and vertices on another, and edges in between.
* There are no edges going between vertices on each side.

![image-20230605155149018](attachments/cs161.assets/image-20230605155149018.png)

* Below is a maximal way to make as many people as possible happy.

![image-20230605155208618](attachments/cs161.assets/image-20230605155208618.png)

#### Solution with Max Flow

![image-20230605155234180](attachments/cs161.assets/image-20230605155234180.png)

![image-20230605155248917](attachments/cs161.assets/image-20230605155248917.png)

* The edges where we have flow are the places where we should make assignments.

#### Why Does it Work?

![image-20230605155423024](attachments/cs161.assets/image-20230605155423024.png)

---

### Application: Assignment Problems

![image-20230605155529243](attachments/cs161.assets/image-20230605155529243.png)

![image-20230605155606273](attachments/cs161.assets/image-20230605155606273.png)

* The numbers in the circle on the left are $c(x)$, how much a student can eat.
* The numbers in the circle on the right are $c(y)$, the capacity of the ice cream.
* The numbers on the edges are $c(x,y)$, the max number of scoops of ice cream $y$ that student $x$ wants.

![image-20230605155745693](attachments/cs161.assets/image-20230605155745693.png)

* The capacity of the edges going into the students are how much they want, and the capacity going out of each tub is the capacity of the ice cream.

![image-20230605155816250](attachments/cs161.assets/image-20230605155816250.png)

#### Why Does This Work?

![image-20230605160025771](attachments/cs161.assets/image-20230605160025771.png)

### Recap

![image-20230605160040902](attachments/cs161.assets/image-20230605160040902.png)

---

## 6/5 L17: More on Ford Fulkerson

![image-20230611151232637](attachments/cs161.assets/image-20230611151232637.png)

* The max flow value is the amount of flow going into the sink vertex $t$.

![image-20230611151542659](attachments/cs161.assets/image-20230611151542659.png)

![image-20230611151633674](attachments/cs161.assets/image-20230611151633674.png)

* The green is the things reachable from $S$, and the orange is the things not reachable. This is our min cut.
  * Remember that a cut is just a partition of vertices

![image-20230611151747743](attachments/cs161.assets/image-20230611151747743.png)

* The cost of the cut refers to the capacities in the original graph, not the flow values.

---

## 6/7 L18: Recap

![image-20230611152412733](attachments/cs161.assets/image-20230611152412733.png)
