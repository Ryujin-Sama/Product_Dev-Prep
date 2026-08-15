### Tool Description Template


| Lever              | get_customer                 | lookup_order                         | process_refund                           |
| ------------------ | ---------------------------- | ------------------------------------ | ---------------------------------------- |
| What it does       | Retrieves verified profile   | Fetches full order details           | Issues a monetary refund                 |
| When to call       | Always call this first       | Requires: get_customer first         | Requires: get_customer then lookup_order |
| What not to use it | Do Not use for order details | Do Not use to issue refunds          | Do Not guess the amount                  |
| What it returns    | name, email, plan, standing  | items, amount, date, status, cust_id |                                          |

### Description overlapping

If you don't tell Claude what a tool isn't for, it will eventually try to use it for that. Explicit boundaries redirect the model

sometimes overlapping is not just from the description itself, we should give proper name for the tools as well.
Self explanatory names reduce selection ambiguity before a single line of description is read. Splitting creates defined input/output contracts.