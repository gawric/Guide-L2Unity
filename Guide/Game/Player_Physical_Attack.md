### Player Physical Attack
The attack time is calculated on the server by type of weapon, for example a sword

<blockquote>
   private float CalculateTimeL2j(float patkSpeed)
   {
       return Math.Max(100, 500000 / patkSpeed);
   }
</blockquote>

In this formula we take the attack speed from the userinfo package and pass it to this code; it returns the attack time of 1200ms

And then the server divides this value in half because the middle of the animation will hit the npc; the rest of the time we spend on returning the animation home
Now we have it: 1200/2 = 600 every 600ms the server will update the parameters of the npc and the player Update and at 1200ms the server will send a new packet if the npc is not dead

### Player Client Animation
1. The most important thing is that the client does not use the same animations and does not use the 1-2-3 combo option one after another. The client uses random and mixes them each time choosing from 3 animations
2. The client also slows down the animation to about 200ms to create the illusion of the heaviness of the object!!!!

### Unity Speed Atk Sync Server L2J
Synchronizing the attack speed of the unity client with the l2j server. I'm using AnimationCurve to simulate the heaviness of a sword swinging at a target.     

During the experiments, I came to the conclusion that float speedAtk = (float)0.3585 / timeAtk; Almost identical to the original client!     
0.07511136f This parameter indicates at what point in time a sharp impact will occur. It fits all swords  
<blockquote>
      private void RecreateAnimationCurve(AnimationCurve animationCurve , float timeAtk , float timeAnimation)  
    {  
        Keyframe startKey = new Keyframe(0f, 0f);   
        Keyframe endKey = new Keyframe(timeAtk, timeAnimation);  
        float speedAtk = (float)0.3585 / timeAtk;  
        Keyframe slowDownAttackKey = new Keyframe(0.07511136f, speedAtk);  
        animationCurve.AddKey(startKey);  
        animationCurve.AddKey(slowDownAttackKey);  
        animationCurve.AddKey(endKey);  
    }  
</blockquote>  