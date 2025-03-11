### Player movement

After analyzing the l2jacis code, here are my conclusions:
PlayerMove->UpdatePosition (repeat 100ms)
<blockquote>
		final double leftDistance = (type == MoveType.GROUND) ? Math.sqrt(dx * dx + dy * dy) : Math.sqrt(dx * dx + dy * dy + dz * dz);
		final double passedDistance = _actor.getStatus().getRealMoveSpeed(type != MoveType.FLY && _moveTimeStamp <= 5) / (1000d / timePassed);
</blockquote>
moveTimeStamp <= 5
until iteration reaches 5 player in walking mode
then we are in running mode
if we reach the goal, we again switch to walking mode

speed walking 78.78
speed run  122.2

Distance = Speed ​​× Time

cancelMoveTask(); <-- switches movement to walking mode


<blockquote>
public void registerMoveTask()
	{
		if (_task != null)
			return;
		
		_blocked = false;
		
		_task = ThreadPool.scheduleAtFixedRate(() ->
		{
			if (updatePosition(false) && !moveToNextRoutePoint())
				ThreadPool.execute(() ->
				{
					cancelMoveTask();
					
					_actor.revalidateZone(true);
					if (!_blocked)
						_actor.getAI().notifyEvent(AiEventType.ARRIVED, null, null);
					else
						_actor.getAI().notifyEvent(AiEventType.ARRIVED_BLOCKED, null, null);
				});
		}, 100, 100);
	}
</blockquote>

if you reach the goal

<blockquote>
	public void cancelMoveTask()
	{
		if (_task != null)
		{
			_task.cancel(false);
			_task = null;
		}
	}
</blockquote>

if the goals are not achieved

<blockquote>
private void moveToPawn(WorldObject pawn, int offset)
	{
		// Get the current position of the pawn.
		final int tx = pawn.getX();
		final int ty = pawn.getY();
		final int tz = pawn.getZ();
		
		// Set the pawn and offset.
		_pawn = pawn;
		_offset = offset;
		
		if (_task != null)
			updatePosition(true);
		
		_instant = Instant.now();
</blockquote>