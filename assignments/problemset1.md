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

1. The completed state definition is below:

   **state**
   a set of Users with

   - a Username
   - a Password

2. The completed actions specification is below:

   **actions**
   register (username: String, password: String): (user: User)
   **requires** no other User with this username or this password exists
   **effects** creates a new User with this username and password and adds it to the set of Users, then returns it

   authenticate (username: String, password: String): (user: User)
   **requires** a User with this username and password exists in the set
   **effects** returns the User with the same username and password

3. The essential invariant is that no two Users have the same Username or the same Password. This is preserved by the requirement for the register action.

4. The extended concept is below:

**concept** Password Authentication

**purpose** limit access to known users

**principle** after a user registers with a username and a password, they can authenticate with that same username and password and be treated each time as the same user

**state**
a set of Users with

- a Username
- a Password
- a valid Email
- a confirmation token SecretToken
- a flag Confirmed

**actions**
register (username: String, password: String, email: String): (user: User, secretToken: String)
**requires** no other User with this username or this password exists in the set
**effects** generates a secret registration token and emails it to the given email, creates a new User with this username, password, email, secret token, and a default Confirmed flag set to False, then adds this new User to the set of Users and returns the User and the token

confirm (username: String, secretToken: String): (user: User)
**requires** a User with this username and secretToken exists in the set
**effects** locates the User with the given username and secretToken, sets the User's Confirmed flag to True and returns the User

authenticate (username: String, password: String): (user: User)
**requires** a User with this username and password exists in the set
**effects** returns the User with the same username and password

## Exercise 3

## Exercise 4
