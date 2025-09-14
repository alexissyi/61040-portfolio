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

**concept** PersonalAccessToken

**purpose** limit access to known users and allow for access customization

**principle** after a user registers with a username and then uploads a set of tokens for specific resources, they can be authenticated for each resource independently and be treated for each resource as the same user across multiple accesses

**state**

a set of Users with

- a Username
- a set of Tokens

a set of Tokens with

- a Resource
- a TokenPassword
- an AccessLevel (READ, WRITE, or ADMIN)

**actions**

register (username: String): (user: User)

**requires** no User with the same username already exists in the set

**effects** creates a new User with the given username and an empty set of Tokens

addToken (username: String, resource: Resource, tokenPassword: String, accessLevel: String)

**requires** a User with the given username exists in the set and does not have a Token for the given resource already associated with it

**effects** adds a Token with the given resource, tokenPassword, and accessLevel to the set of Tokens for the User with the given username

authenticate (username: String, resource: Resource, tokenPassword: String, accessLevel: String): (user: User)

**requires** a User with the given username exists in the set and has a Token associated with it for the given resource and accessLevel with the same tokenPassword

**effects** returns the User with the given username that has a Token for the given resource and accessLevel with the same tokenPassword

The main difference between the PersonalAccessToken concept and the Password concept is that the PersonalAccessToken concept allows for multiple tokens to be attributed to the same user for access to different resources, with varying levels of access, while the Password concept links a user to a single resource.

## Exercise 4

### Concept 1: URL Shortener

**concept** URLShortener

**purpose** turn long URLs into short URLs

**principle** a user can upload a long URL and get a more readable short URL that points to the same page

**state**
a set of ShortenedURLS with

- an original LongURL
- a ShortURL
- a Suffix

**actions**

generateSuffix(): (suffix: String)

**requires** nothing

**effects** automatically generates a URL suffix and returns it

shortenURL(longURL: String, suffix: String)

**requires** no ShortenedURL exists with the given longURL and a shortURL with the given suffix

**effects** generate a shorter URL (thirty characters or less) with the provided suffix and create a new ShortenedURL with the suffix, new ShortURL and the given LongURL

retrieveURL(shortURL: String): (longURL: String)

**requires** a shortenedURL exists with the given shortURL

**effects** finds the ShortenedURL with the given shortURL and returns the LongURL associated with it

### Concept 2: Billable Hours Tracking

**concept** BillableHoursTracking

**purpose** automate record keeping of billable hours

**principle** by having each employee mark the beginning and end of their work session, can track the total hours worked by every employee for each company

**state**

a set of Employees with

- an identifier EmployeeID
- a MaxSessionTime
- a set of FinishedSessions
- a set of ActiveSessions

a set of FinishedSessions with

- a StartTime
- an EndTime
- a Description

a set of ActiveSessions with

- a StartTime
- a Description

**actions**

createEmployee(employeeID: String, maxSessionTime: float)

**requires** no Employee exists with the given employeeID

**effects** create a new Employee with the given employeeID and maxSessionTime and empty sets of FinishedSessions and ActiveSessions

updateMaxSessionTime(employeeID: String, maxSessionTime: float)

**requires** an Employee exists with the given employeeID

**effects** updates the employee's MaxSessionTime to be the given maxSessionTime

startSession(employeeID: String, startTime: Time, description: String)

**requires** an Employee exists with the given employeeID, that Employee has no ActiveSessions, and their most recent FinishedSession was completed at or before startTime

**effects** creates a new ActiveSession with the given startTime and description

endSession(employeeID: String, endTime: Time)

**requires** an Employee exists with the given employeeID and that Employee has an ActiveSession with a startTime that is not more than maxSessionTime earlier than endTime

**effects** takes the Employee's ActiveSession and creates a new FinishedSession with the same Description and StartTime, along with the provided endTime, and deletes the ActiveSession

### Concept 3: Conference Room Booking

**concept** ConferenceRoomBooking

**purpose** track room reservations and prevent conflicts

**principle** by having users create room reservations, can track which rooms are free to reserve and which ones are booked

**state**

a set of Rooms with

- a RoomNumber
- a MaxCapacity
- a MinCapacity

a set of Reservations with

- a Room
- a Timeslot
- a person making the reservation User
- a count of attendees Attendance

a TimeSlot with

- a StartTime
- an EndTime

**actions**

createRoom(roomNumber: String, minCapacity: int, maxCapacity: int)

**requires** no other Room with the same roomNumber exists, minCapacity and maxCapacity are both nonnegative integers and maxCapacity >= minCapacity

**effects** creates a new Room with roomNumber, minCapacity, maxCapacity

reserveRoom(user: User, roomNumber: String, timeslot: TimeSlot, attendance: int)

**requires** a Room exists with the given roomNumber, the room's minCapacity <= attendance <= the room's maxCapacity, and no other Reservation for that Room has an overlapping timeslot

**effects** creates a new Reservation for that Room with the given roomNumber, user, timeslot and attendance

cancelReservation(user: User, room: Room, timeslot: TimeSlot)

**requires** a Reservation exists with the given user, room and timeslot

**effects** deletes that Reservation

### Notes

For Concept 2, one invariant is that there is never more than one ActiveSession per Employee, and this is preserved through all actions.
