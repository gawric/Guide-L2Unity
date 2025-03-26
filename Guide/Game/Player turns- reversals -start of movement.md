### Player turns - reversals - start of movement

## 1.Turns
Turns in front of the player are made without softening the movement and at a very fast speed.

## 2.Reversals
Turning in the opposite direction is very slow > my working scheme is turning towards the target at a speed of 0.06f and at this point in the movement the player slows down to 1f. The movement slows down exactly until the end of the turn, this must be counted and taken into account

## 3.Start of movement
The moment of the start is the most difficult moment. At the moment of starting just forward, we take into account in the code that the player starts, and does not switch while running. During the start, we slow down the animation by 0.40f and switch to a speed of 2.3 mert per second, we move exactly 400ms and after that we switch to running if we don't do this, there will be a very noticeable shift in speeds. And if we have our backs to the target during the start, we also need to slow down the turn and slow down the speed of movement to 1.8ms...... It's not that simple




All this helped me achieve almost 90% synchronization with the l2j server, and now the attack and conversation with the npc appear as on an orginal client.

