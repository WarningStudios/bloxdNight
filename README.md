# bloxdNight

This is the code for night in Bloxd.io!
Copy all the following in world code!
More info at https://discord.com/channels/804347688946237472/1341451454048899144/threads/1376228922022301736

```
  let spawnMaxHeight = 300
  /*
  	Important for void worlds. 
  	Mobs will spawn from this height to 500 blocks below.
  */
  
  let sleepRate = 0.5
  /*
  	The rate of players who have to sleep to skip night.
  */
  
  
sleeping=[],spawnedMobs=[];let nightMobs=["Draugr Zombie","Draugr Zombie","Draugr Skeleton"],skySpeed=5e5,inclinationList=[-.6,-.575,-.55,-.525,-.5,-.45,-.4,-.35,-.3,-.25,-.2,-.15,-.05,0,.05,.1,.15,.2,.25,.3,.35,.4,.45,.475,.5],intervals=[3e4,3e4,3e4,3e4,3e4,15e3,15e3,15e3,15e3,15e3,skySpeed/10,skySpeed/10,skySpeed/10,skySpeed/10,skySpeed/10,skySpeed/10,skySpeed/10,skySpeed/10,skySpeed/10,skySpeed/10,15e3,15e3,15e3,15e3,15e3];function raycast(e,t){e[1]++;let i=!1;for(let a=0;a<100&&0==i;a++){let a=api.raycastForBlock(e,t);if(null!=a){if("Unloaded"==api.getBlock(a.position))return;return i=!0,a[0]+=.5,a[2]+=.5,a.position}e=[e[0]+5*t[0],e[1]+5*t[1],e[2]+5*t[2]]}}function spawnMob(e,t,i){try{let a=raycast([t+.5,300,i+.5],[0,-1,0]);a[1]++,spawnedMobs.push(api.attemptSpawnMob(e,a[0]+.5,a[1],a[2]+.5))}catch(e){}}function setHour(e){skyIndex=Math.max(0,Math.min(e,23))}skyTime=0,skyIndex=12,tick=e=>{for(skyTime+=e;skyTime>=intervals[skyIndex];)skyTime-=intervals[skyIndex],skyIndex++,skyIndex>=intervals.length-1&&(skyIndex=0,skyTime=0);let t=skyTime/intervals[skyIndex],i=inclinationList[skyIndex],a=i+(inclinationList[skyIndex+1]-i)*t;api.getPlayerIds().forEach((e=>{if(api.setClientOption(e,"skyBox",{type:"earth",inclination:a,turbidity:1,luminance:.75,azimuth:0,infiniteDistance:3,vertexTint:[255,255,255]}),skyIndex>23||skyIndex<4){if(Math.random()>.95){let t=Math.floor(120*Math.random()-60),i=Math.floor(120*Math.random()-60),a=api.getPosition(e),s=a[0]+t,n=a[2]+i;spawnMob(Math.random()>1/3?nightMobs[0]:nightMobs[2],s,n)}}else{for(const e of spawnedMobs)e&&api.checkValid(e)&&api.killLifeform(e);spawnedMobs=[]}}))};let recipesForClock=[{requires:[{items:["Gold Bar"],amt:2}],produces:1}];function onPlayerAltAction(e,t,i,a,n){let l=api.getEntityName(e);if(null!=api.getHeldItem(e)||"Air"!=n)if(n.includes("Bed")&&"Bedrock"!=n)if(skyIndex>=23||skyIndex<5){sleeping.includes(l)?(api.removeEffect(e,"Frozen"),delete sleeping[sleeping.indexOf(l)]):(api.sendMessage(e,"Sleeping... "),sleeping.push(l),api.applyEffect(e,"Frozen",null,{displayName:"Sleeping",icon:"Red Bed"}));let t=sleeping.filter((e=>null!=e)).length;if(t>=api.getNumPlayers()*sleepRate){for(s of(api.broadcastMessage("Sleeping through this night..."),skyIndex=6,sleeping))api.checkValid(api.getPlayerId(s))&&s&&api.removeEffect(api.getPlayerId(s),"Frozen");sleeping=[]}else{let e=Math.floor(api.getNumPlayers()*sleepRate)-t;api.broadcastMessage("The night will be skipped if "+e+" more players are sleeping")}}else api.sendMessage(e,"You can sleep only at night.");else"Yellow Sticky Paintball Explosive Item"==api.getHeldItem(e)?.name&&api.sendMessage(e,"Time: "+skyIndex,{color:"yellow"})}function onPlayerAttemptCraft(e,t,i){let a=!0;try{onPlayerAttemptCraft1}catch(e){a=!1}if(a)var s=onPlayerAttemptCraft1(e,t,i);if("Yellow Sticky Paintball Explosive Item"==t){if(!api.hasItem(e,"Yellow Sticky Paintball Explosive Item")){let t=[{str:api.getEntityName(e)+" has made the advancement "},{str:"[Who needs that?]",style:{color:"lime"}}];api.broadcastMessage(t)}return api.giveItem(e,"Yellow Sticky Paintball Explosive Item",null,{customDisplayName:"Clock",customDescription:"Right click to get time."}),api.removeItemName(e,"Gold Bar",2),api.removeItemName(e,"Iron Bar",3),"preventCraft"}return s}function onPlayerJoin(e){recipesForClock=[{requires:[{items:["Gold Bar"],amt:2},{items:["Iron Bar"],amt:3}],produces:1}],api.editItemCraftingRecipes(e,"Yellow Sticky Paintball Explosive Item",recipesForClock);let t=!0;try{onPlayerJoin1}catch(e){t=!1}t&&onPlayerJoin1(e)}
```
