## Concept Questions

1. Contexts are for restricting the format of the strings generated. For example, you might only want English names, or you only want addresses. In the URL shortening app, the context is web URLs.

2. The NonceGeneration concept needs to store used strings to make the generate action possible. Generating strings that haven't been generated requires some way to know which strings that have already been generated.

The proposed implementation requires some kind of implicit ordering of the strings in each context. Then, every time generate is called, the next string in the ordering would be returned. The set of used strings for a given counter value n is the set consisting of strings #1, #2, . . ., through #n in the ordering of the strings in the context.

3. One advantage is that these types of shortened URLs are easier for the user to remember (it's easier to remember dictionary words than an arbitrary combination of letters and numbers). One disadvantage is that these shortened URLs might imply something false about the target URL based on the dictionary words chosen. For example, a url that is shortened to dolphinhat.com arbitrarily might lead someone to think the url leads to a web page related to hats for dolphins, even if that's not the case.

To realize this idea for the NonceGeneration concept, we would need to modify the generate action to only use common dictionary words when generating strings. We might not need to modify the NonceGeneration concept at all though -- we could just define the Context as the set of web urls that use common dictionary words.

## Synchronization Questions

1.

## Extending the Design
