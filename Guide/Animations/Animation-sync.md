# Unity Client & L2J Server Animation Synchronization

This document describes the technical implementation of synchronizing Unity client-side animations with the L2J server's skill timing logic.

## 1. Data Acquisition (Server-to-Client)

The synchronization process is triggered by the server-side `MagicSkillUse` packet. 

* **Critical Parameter:** `hittime = ReadI();`
* **Objective:** The server defines the exact window (in milliseconds) within which the hit must be registered on the client (e.g., **1000ms**).

## 2. Animation Preset Resolution

To match the server's skill ID with a specific visual asset, the client performs a lookup in the `AnimationCombo.data`:

1.  **Base Key:** `SkillgrpTable.Instance.GetAnimComboBySkillId(...)` retrieves a base animation key (e.g., `SpAtk01`).
2.  **Weapon Suffix:** The base name is modified according to the character's current equipment:
    *   `1HS` — One-handed sword.
    *   `2HS` — Two-handed sword.
3.  **Result:** The final animation name becomes `SpAtk01_2HS`.

## 3. Playback Initialization

The animation is triggered via the `AnimationManager`, and the server-defined hit time is stored within the Animator's parameters for later processing.

```csharp
// Execute crossfade to the resolved animation
IAnimatorManager manager = animationManager.AsyncPlayAnimationCrossFade(objectId, animName + "_");
```

// Store the server's hit time (ms) in the Animator controller
animator.SetInteger("sptimeatk", serverTimeMs);

## 4. StateMachineBehaviour Logic

The core synchronization happens inside a custom `StateMachineBehaviour` using the `OnStateEnter` method.

### Keyframe Events
Each animation clip must contain the following events for precise tracking:
* **OnAnimationHit**: Start of the hit phase.
* **OnAnimationAttackHitEnd**: End of the hit phase.
* **OnEndAnimation**: Clip termination.

### Playback Speed Calculation
To ensure the visual hit occurs exactly at the server's timestamp, we dynamically adjust the `animator.speed`.

```csharp
override public void OnStateEnter(Animator animator, AnimatorStateInfo stateInfo, int layerIndex)
{
    // 1. Retrieve clip timing metadata from cache
    float startHit = AnimationDataCache.GetEventTimeByName(animator, motionName, eventStartHitName);
    float endHit = AnimationDataCache.GetEventTimeByName(animator, motionName, eventEndHitName);
    
    // Calculate the midpoint of the 'hit' window within the source clip
    _eventHitTimeInClip = (startHit + endHit) / 2f;

    // 2. Process Server Timing
    int serverTimeMs = animator.GetInteger("sptimeatk");
    
    // Apply network/processing latency compensation
    float compensation = 0.08f;
    _serverHitTime = (serverTimeMs / 1000f) - compensation;

    // Clamp values to prevent zero or negative speeds
    if (_serverHitTime < 0.1f) _serverHitTime = 0.1f;

    // 3. Dynamic Speed Adjustment
    // Formula: (Internal Clip Hit Time) / (Target Server Hit Time)
    float startSpeed = _eventHitTimeInClip / _serverHitTime;

    animator.speed = startSpeed;
}
```

UI Inventory Удаление   
<img src="https://i.postimg.cc/Dz0xF8XF/sync-atk.png" alt="Preview0" style="max-width: 40%; height: auto;">     
