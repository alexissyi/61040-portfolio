## Exercise 1

1. Invariant 1: for any Request, the count requested is nonnegative (request.number >= 0)

Invariant 2: every Purchase is for an Item within the set of Requests

Invariant 2 is most important. The whole point of a gift registry is so that you only get the gifts that you actually want. The action most affected by this invariant is the "purchase" action; this action preserves the invariant by requiring that the Item to be purchased has a requested amount that is at least the amount we're attempting to purchase. By definition this requires the Item to be in the set of Requests.

2. The "removeItem" action could potentially break Invariant 2. This happens if some Purchases are made for an Item, then the registry owner removes that Item. This problem can be fixed if the removeItem action requires that no Purchases have been made for an Item before it can be removed.

3. The current spec allows the registry to be open and closed repeatedly. One reason to allow this is that often a gift registry is developed over time, and the owner might want to update it as they have new ideas. Allowing the owner to close the registry allows them to make changes, including deleting items they no longer want that no one has purchased yet, without potential purchasers changing the registry at the same time as the owner. Another reason to allow this is if the owner wants to reuse the registry for multiple events.

4. In practice, this wouldn't matter very much, unless the owner is super sensitive about their registry data being out there. If the owner doesn't want people to make purchases, they can just close the registry.

5. The owner is often interested in knowing which requests have been completely filled. The purchaser is probably going to be interested in knowing which requests are still available to fill (so requests that haven't been completely filled).

6. We would need to add a "visibleToOwner" attribute to every Registry that the owner can decide to set to False or True. Default should probably be True. Then for any queries from the owner on the concept we would need to check whether the visibleToOwner attribute is True before revealing anything to the owner.

7. Unique SKU codes are more efficient and easy to compare than listing out all of an Item's name, description and price. Some Items might have identical or extremely similar names, descriptions and prices.

## Exercise 2

## Exercise 3

## Exercise 4
