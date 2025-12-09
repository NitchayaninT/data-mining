# Association Analysis
Given a set of transaction, find rules to predict the occurrence of an item **based on the occurence of the others**
![[Pasted image 20251201125758.png]]

Association rules example
- For example, **{beer, bread} -> milk**
We see that a customer has bought beer & bread, it is likely that she will also buy milk

## Definition
**A-> B (if A then B)**
- A in the subset of I, B in the subset of I, A AND B = empty set
- A = antecedent
- B = consequence/conclusion
- It is a descriptive data mining task, it is not to predict conclusion (B) in the same way as predicting target class in classification. 
- Association rule tells us that **if A occurs, B is also likely to occur** (based on **evidence** in data set)

## Input Format
![[Pasted image 20251201130221.png]]

- **attributes** are treated as **asymmetric attribute**
- In this example, there are so many items in the supermarket. we usually **focus on items being bought by customers** (when attribute values = 1, or true)

![[Pasted image 20251201130458.png]]

- attributes are treated as **symmetric attribute**
- number of issues are usually small. We are **interested in issues being both agreed and disagreed** by voters (when attribute values = 0, false and 1, true)
- Shorter words : interested in both agree & disagree

## Support & Confidence
==**Support or coverage**==
- Fraction of transactions containing itemset(s), for example
	- Support({beer}) = 1/3
	- Suppoer({bread} U {coke}) = 2/3
- In association analysis, **A U B** means **A and B co-exist**. its equivalent to A AND B in math

Given an association rule **A->B**
- **Support(A->B)** = support(A U B) = **P(A AND B)**
	- Contains both A and B
- **Confidence(A->B)** = support(A U B) / support(A) = **P(B | A)**
	- Prob of B given A, Prob that B appears in transactions containing item A

Goal of association analysis = ==**find strong association rules**==
- support(A->B) >= minSupport
- confidence(A->B) = minConfidence
Rules with **too low support** = may occur by chance & are likely to be uninteresting 
Confidence **measures the reliability** of the inference made by the rule

**Typical processing**
- **Phase 1** : find all frequent itemsets that pass minSupport criterion (FP-Growth)
- **Phase 2** : **generate rules** by permuting frequent itemsets & keep rules that **pass minConfidence** criterion

## Phase 1 : Finding Frequent Itemsets
### FP-Growth
![[Pasted image 20251201143412.png]]

#### FP-tree construction
- Reads each transaction, from ID = 1 to last, which values are inside the transaction?

**reading TID = 1**
![[Pasted image 20251201143559.png]]

**reading TID = 2, 3**
![[Pasted image 20251201143630.png]]

- **TID = 2**, update B +=1, D +=1. Update pointer D to be leaf node of B & update count (**support**) at each node
- **TID = 3**, update B+=1, C+=1. Update pointer C to be leaf node of B & update count at each node

**reading TID = 4, 5**
![[Pasted image 20251201144124.png]]

Do this until **TID = 9**
![[Pasted image 20251201144514.png]]

- Record reverse order of header table
	- Check pattern from FP tree. Check prefix paths **(count from suffix node to nodes above, or to "prefixes")**
	- Support count = **number in the suffix**

![[Pasted image 20251201145017.png]]

**Consider suffix E's prefix paths & construct header table
- 2 B's in E's prefix paths
- 2 A's in E's prefix paths
- 1 C in E's prefix paths
- theshold is 2, so C is pruned out

**Consider suffix D's prefix paths & construct header table**
- 2 B's in D's prefix paths
- 1 A in D's prefix paths
- treshold is 2, so A is pruned out

Do the same for the rest, **Consider suffix C's and A's prefix paths**
![[Pasted image 20251201145726.png]]

**Results**
- Frequent 2-itemsets : (AB : 4), (AC : 4), (AE : 2), (BC : 4), (BD : 2), (BE : 2)
- Frequent 3-itemsets : (ABC : 2), (ABE : 2)
## Phase 2 : Generating Association Rules
- For each frequent itemset (or frequent pattern FP), **Permute items to get association rule** and keep only rules that pass minConfidence
- For example : possible rules for frequent itemset BCE
	- BC -> E, BE -> C, CE -> B, E -> BC, C -> BE, B -> CE
- **conf(rule) = support(rule) / support(premise)**
	- example BC -> E : if we know a customer has bought B and C (premise), we can be confident (or not) that she'll buy E (conclusion)
	- Conf(ABC -> D) >= Conf(AB -> CD) >= Conf(A->BCD)
	- **Less Premise (more support)** -> less RULE confidence, more premise -> more RULE confidence
## Evaluating Association Rules
- Support
- Confidence
- Lift
### Lift
Lift tells how much more likely the conclusion happens _when the premise happens_, compared to random chance.
It measures **how strong the relationship** is between items.
- Independent items VS confidence
- From confidence (A->B) = P(B | A) = P(A AND B) / P(A)
- If A and B are *independent*, then P(A AND B) = P(A) x P(B)
- **Lift (A->B) = confidence (A->B)/support(B)**
			**= P(A AND B) / (P(A) x P(B))**
	- Lift(A->B) < 1 implies ==negative association==. can be interpreted as negating(~) one side of the rule. eg : customers buy either item
	- Lift(A->B) = 1 implies ==no association (independent)==. example : Eating omelette does NOT change the chance of choosing muesli yogurt, they're independent, like flipping a coin
	- Lift(A->B) > 1 implies ==positive association==, conclusion **happens more often with the premise** than in general. indicates that premise and conclusion occur together -> eg. customer buy both items

