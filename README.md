# QuestGameEvents

GameEvents library in aslx for [Quest](http://textadventures.co.uk/quest)

There's a great page in the documentation about [turn based events](http://docs.textadventures.co.uk/quest/guides/turn_based_events.html).

Here's a library with tabs to support that.

To use it

1.  Create two rooms "dead_events" and "active_events" with no exits into or out of them
2.  Add an integer attribute called "turn" to your main game object, set it to 0
3.  Create events that you want to happen without special prompting in "active_events" by
    * right-click, new object > change drop-down under "Type" from "None" to "Game Event"
    * set the number of turns after the game starts that you want the event to occur
    * add a script to implement the behaviour of the event
    * check "auto destroy" if you only want the event script to run once
4.  Create events that you want to happen contingent on previous events under "dead_events"
    * right-click, new object > change drop-down under "Type" from "None" to "Game Event"
    * set the number of turns after being triggered that it should start
    * add a script
    * check "auto destroy" if needed
    * trigger it by:
         * find an event in 2) above and set the "Next event:" drop-down to point to this one OR
         * write a script that uses one of the functions below

When your event is scheduled as a "next event", the "turns" setting is the number of turns after being scheduled that it runs. Of course if you want it to run immediately after its pre-cursor event triggers it you can just make its "turns" equal to 1 or 0.

There are 3 functions you can use:

    ScheduleGameEvent(some_event)

The event will occur, after some_event.turn turns have passed. (It's moved to active_events)


    UnscheduleGameEvent(another_event)

The event will not occur. (It's moved to dead_events).


    StageEvent(super_event)

If done is true, does nothing. If done is false the event immediately occurs. Its script is run and its done flag is set true.

Note that after an event occurs the done flag is set. Under the hood, StageEvent is used to make events occur. You can test this flag to see if an event has been completed or not.

CODE: SELECT ALL
if (not balloon_inflating.done) {
  msg ("I shall use my inflate-o-matic!")
}


If you call ScheduleEvent on an event it will not alter the done flag. If done is true, the event will be moved to active_events but it will never occur. If you want an event to occur, but somehow done is set, then manually set it to false before calling ScheduleEvent. UnscheduleGameEvent likewise does not alter the done flag.

## Why couldn't you make the library automatically add the rooms and so on?

I know steps 1 - 3 above are a pain. But there's no way to be sure that someone hadn't already got a dead_events or game.turn. Hopefully the sample game makes it clear.

It's most likely riddled with bugs: I'll see how excited I am about fixing them, but let me know.
