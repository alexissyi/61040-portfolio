## Concept Questions

1. Contexts are for restricting the format of the strings generated, both in type and in specifics (by tracking the previously generated strings). For the type of the string, you might only want English names, or you only want addresses. In the URL shortening app, the context is web URLs, specifically web URLs with shortURLBase as the beginning of the URL.

2. The NonceGeneration concept needs to store used strings to make the generate action possible. Generating strings that haven't been generated requires some way to know which strings that have already been generated.

   The proposed implementation requires some kind of implicit ordering of the strings in each context. Then, every time generate is called, the next string in the ordering would be returned. The set of used strings for a given counter value n is the set consisting of strings #1, #2, . . ., through #n in the ordering of the strings in the context.

3. One advantage is that these types of shortened URLs are easier for the user to remember (it's easier to remember dictionary words than an arbitrary combination of letters and numbers). One disadvantage is that these shortened URLs might imply something false about the target URL based on the dictionary words chosen. For example, a url that is shortened to dolphinhat.com arbitrarily might lead someone to think the url leads to a web page related to hats for dolphins, even if that's not the case.

   To realize this idea for the NonceGeneration concept, we would need to modify the generate action to only use common dictionary words when generating strings. We might not need to modify the NonceGeneration concept at all though -- we could just define the Context as the set of web urls that use common dictionary words.

## Synchronization Questions

1. The targetURL argument does not appear in the first sync because it is not used for the generate action. The behavior of the generate action does not depend on the value of targetURL that is passed to the shortenURL action. In the second sync however, the specific value of targetUrl is relevant to the behavior of the register action, which uses that value.

2. Sometimes names can mean different things in different contexts. In this case we care to know explicitly that there are two separate entities with the same name; omitting in this context would have the effect of making the reader think that both names mean the same thing in both of their respective contexts.

3. The request action is not included in the third sync because the action in the then clause does not depend on the arguments/results of the request action. In the second sync, the then clause depends on two arguments from the request action, and in the first sync, the then clause depends on one argument from the request action.

4. In the first sync, the generate action would be called with "bit.ly" as the argument to the context parameter, and there would be no shortUrlBase argument to the request action. In the second sync, shortUrlBase would be removed as a parameter.

5. This sync deletes the corresponding URL pair when the short URL expires:

   **sync** delete

   **when**
   ExpiringResource.expireResource(): (resource)

   **then**
   UrlShortening.delete(shortUrl: resource)

## Extending the Design

1.  ### Concept 1: CountAccesses

    **concept** CountAccesses \[Resource\]

    **purpose** count the number of times a resource is accessed

    **principle** each time a resource is accessed, its accesses count goes up

    **state**

    a set of Resources with

        a count Accesses

    **actions**

    trackResource (resource: Resource)

    **effect** adds this resource to our set and sets Accesses for this resource to 0

    recordAccess (resource: Resource)

    **requires** this resource is in our set

    **effect** increases Accesses by 1

    viewAccess (resource: Resource): (numAccesses: int)

    **requires** this resource is in our set

    **effect** returns resource.Accesses

    ### Concept 2: ResourceOwner

    **concept** ResourceOwner \[Resource, User\]

    **purpose** to identify one user to whom each resource belongs for special privileges

    **principle** each resource is associated with a user as an owner

    **state**

    a set of Resources with

        a user Owner

    **actions**

    giveOwnership(resource: Resource, user: User)

    **requires** this Resource does not already have an Owner, and this user is not already an Owner for a different Resource

    **effect** adds this user as the Owner of this Resource in our set

    verifyOwnership(resource: Resource, user: User)

    **requires** this User is the Owner for this Resource

    **effect** nothing
