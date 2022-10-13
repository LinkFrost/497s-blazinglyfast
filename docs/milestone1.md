# Team Blazingly Fast

## App Name - Milestone 1

Version 0.1

---

### Application Domain

The application is an anonymous feedback collection website where a user can create a room accessible through a unique url that contains either a message board, poll, or both. The user can then share the generated url to as many people as they’d like and have the room expire after a set amount of hours.​ After the room expires, the contents that were recorded throughout its duration are compiled and sent to the owner’s email. The room should be able to support a large number of users at once and update live when new poll responses or messages are sent. Once the room expires, it will no longer accept messages/votes and enter a read only state. This application could be part of a larger idea and extended beyond our implementation.

### Data Model

The application has the following date types:

**Users:** The users object stores the information for each signed up user

**Rooms:** A room object contains all the relevant information pertaining to a room, such as its title and type (poll or messages).

**Messages:** Stores every message that is sent to a room.
Polls/Poll Options: Stores polls and their related options for a room.

**Expired Rooms:** Keeps track of which rooms are expired.
Room Visuals: Contains the visualized versions of expired rooms.

**Moderator:** Has a list of banned words that are referenced after stemming and tokenizing each comment.

**Site Health:** Various website statistics used for monitoring.

_Below is a diagram for each data model in JSON with examples and how they related to each other._

![](./data_model.png)

### Services

**Rooms Service:** The client will communicate with this service to create a new room. The database will keep track of all rooms (expired or not), and only allow new room creation from authenticated users.

**Messages Service:** The client will communicate with this service to create a new message. The database will keep track of all messages that have been moderated (after creation), and the amount of votes associated with each.

**Polls Service:** This service is responsible for creating a new poll. The database will store the options and vote counts for each option.

**Moderator service:** The moderator service stores a list of banned words. Everytime it receives a message creation event, it will first check that message against the list before approving it and sending it back to the messages service and client.

**Email service:** This service is responsible for notifying a room owner about all the relevant room data when one of their rooms expires. This data is retrieved from the visualizer service which is then processed and sent to the owner’s email.

**Users service:** This service is responsible for creating a new user or retrieving existing user details. The database stores all the info related to users. The authentication service will have to communicate here.

**Authenticator Service:** The authenticator handles authentication and logging in users as well as making sure that a user session is valid when attempting to access secure pages.

**Expiration Service:** This service keeps track of when a room is created and when it expires. When a room expires, it notifies the visualizer service, rooms service, query service, and the site health service.

**Site Health Service:** This service receives a majority of the events being emitted by other services in order to keep track of various site related statistics. These can then be viewed on the client side on a “site health page”. This would monitor things like number of users, number of open rooms, HTTP error rate, etc.

**Visualizer Service:** This service is responsible for processing when a room expires. It will take all the data relevant to the expired room and format it appropriately in a clean, visualized image format. This data is utilized by the email and user service.

**Query Service:** The query service is used to get data back to the client to reduce the number of network calls to each service. Storing the data in one service can result in much fewer requests and increase speed as shown in our lecture example. This service will store the data which results in data duplication, but is an expected result of using microservices.

**Vote Service:** This service handles voting for messages and polls. On the client side, a user can upvote or downvote a message in a room. This will be sent to the vote service, which will handle updating a messages vote count property. This is then sent to the messages service to update that data, and to the query service to update the client side UI. The same thing is done when a user votes on a poll option. This service also communicates with site health to keep track of the number of votes.

_Below is a diagram showing all of the services and how they interact with the event bus and client._

![](./services.png)

### Endpoints

### Events and Communication

### Meeting Notes
