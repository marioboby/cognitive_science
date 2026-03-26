---
{"dg-publish":true,"permalink":"/reasoning/before-mid/lecture-4-examples-on-chaining/"}
---


| Feature            | Forward Chaining                                                                                                                | Backward Chaining                                                                                                                         |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| **When Suggested** | All or most data is given; large number of potential goals with few achievable; difficult to formulate a goal.                  | Goal or hypothesis is given or easily formulated; large number of rules but goal prunes search space; data must be acquired as necessary. |
| **Advantages**     | Simple.                                                                                                                         | Search is goal directed, only necessary rules are applied.                                                                                |
| **Disadvantages**  | Many applicable rules at each stage (conflict resolution needed); process not directed towards a goal (stop condition unknown). | A goal has to be known.                                                                                                                   |

---

## Conflict Resolution Strategies

Conflict resolution is the method for choosing which rule to fire when more than one rule (the conflict set) can be fired in a given cycle.

|Strategy|Description|
|---|---|
|**First Applicable**|Firing the first applicable rule if the rules are in a specified order.|
|**Most Specific**|Choosing the rule with the most conditions, based on the assumption that a specific rule processes more information.|
|**Least Recently Used**|Choosing the rule with the earliest time or step mark, which marks the last time it was used.|
|**Highest Priority**|Choosing the rule with the highest 'weight' assigned to it.|
|**Most Recently Entered**|Firing the rule whose antecedent uses the data most recently added to the database.|