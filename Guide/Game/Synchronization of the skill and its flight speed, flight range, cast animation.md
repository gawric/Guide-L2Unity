### Synchronization of the skill and its flight speed, flight range, cast animation


According to my observations, the further away the object is, the slower it casts the spell, but it doesn't take much time, except for slowing down, it can play the first animation and wait for a call from the code to continue if the object is very far away. I also noticed that the further away an object is, the slower it moves, but it also has a deceleration limit, and the closer it gets, the faster it moves. Therefore, I conclude that there is a file with different speeds depending on the distance to the object.

![Sync WindStrike Anim+Fly!](https://i.ibb.co/XfZDM40B/Sync-Hit-Time.png) 