## mafia-chatbot

This is a Slack chatbot that plays Mafia. Once installed in a workspace, a game can be started with
`/startgame`. 
Players then follow prompts in the chat to accuse and convict people, and the game continues.

NOTE FOR DRAFT SUBMISSION: because it would be quite impractical for you, the evaluator, to setup your own slack workspace
and attempt to mirror the setup of our workspace, you can take a look at the code in this submission instead.
Because we are still developing the project in our slack workspace, it wouldn't be productive
for you to try playing it there either (not to mention you need at least 5 people to play and you probably don't want to create a bunch of dummy slack accounts like we did).

TL;DR: too difficult to submit something you can actually see functionality of in action. Please look at our code instead :)






## General Game Ideas

### Commands triggered from channel
1. /startgame command - takes all users in the channel and makes a game with them, DMing each user their role. Options: timed and untimed modes
1. /getrules command - prints rules out to the channel
1. /accuse - accuse a user of being the mafia. Puts a message in the channel that has a button that, when clicked, registers a "second" on the accusation
1. /endgame command - ends a game if there is one currently going


For the individual DMs with the roles, Mafia, doctors, etc, they get DMs asking who they want to pick and they @-mention a person

Voting occurs in the following way: the bot puts a message in the channel and counts the first "yes" or "no" from each person as their vote

or have the voting occur via slack app buttons if we can do that


### Endpoints for commands
- /startgame -> /cmd/startgame
- /getrules -> /cmd/getrules
- /accuse -> /cmd/accuse
- /endgame -> /cmd/endgame


#### Mafia Consensus

Once it is time for the mafia to decide who to kill, a countdown starts in their group channel.
They must arrive at a consensus by the end of the countdown. Mafia members indicate who they want to 
kill by using `kill @person-mention` in the chat. If 100% of mafia members vote for the same person, the voting ends
immediately. At the end of the countdown, the person to kill is determined as follows:
- if there was at least 1 kill mention, the person with the most kill mentions dies
- if there were no kill mentions, nobody dies

Mafia members can change their vote on who to kill by submitting a new kill mention - only the most recent kill mention
for each mafia member is saved and used in the final consideration.


#### Villager Accusing/Consensus

During the day, villagers can accuse someone using the `/accuse @Name` command. Once there has been an accusation, a different user
must use `/second @Name` to second the accusation. As soon as there has been an accusation and a second, a countdown starts. Villagers vote to kill
by writing `vote yes` in the chat and vote not to by writing `vote no` in the chat. Villagers can vote as many times as they like and only the last vote is counted.

Voting terminates in one of two ways:
- if all the villagers vote yes
- when the countdown expires

If the second option is taken, the accused is killed if the majority of the votes cast indicate that they should be.
