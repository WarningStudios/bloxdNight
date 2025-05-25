# bloxdNight

This is the code for night in Bloxd.io!



  let spawnMaxHeight = 300
  /*
  	Important for void worlds. 
  	Mobs will spawn from this height to 500 blocks below.
  */
  
  let sleepRate = 0.5
  /*
  	The rate of players who have to sleep to skip night.
  */
  
  
  sleeping=[],spawnedMobs=[];let nightMobs=["Draugr Zombie","Draugr Zombie","Draugr Skeleton"],skySpeed=5e5,inclinationList=[-.6,-.575,-.55,-.525,-.5,-.45,-.4,-.35,-.3,-.25,-.2,-.15,-.05,0,.05,.1,.15,.2,.25,.3,.35,.4,.45,.475,.5],intervals=[3e4,3e4,3e4,3e4,3e4,15e3,15e3,15e3,15e3,15e3,skySpeed/10,skySpeed/10,skySpeed/10,skySpeed/10,skySpeed/10,skySpeed/10,skySpeed/10,skySpeed/10,skySpeed/10,skySpeed/10,15e3,15e3,15e3,15e3,15e3];function raycast(e,t){e[1]++;let i=!1;for(let n=0;n<100&&0==i;n++){let n=api.raycastForBlock(e,t);if(null!=n){if("Unloaded"==api.getBlock(n.position))return;return i=!0,n[0]+=.5,n[2]+=.5,n.position}e=[e[0]+5*t[0],e[1]+5*t[1],e[2]+5*t[2]]}}function spawnMob(e,t,i){try{let n=raycast([t+.5,300,i+.5],[0,-1,0]);n[1]++,spawnedMobs.push(api.attemptSpawnMob(e,n[0]+.5,n[1],n[2]+.5))}catch(e){}}function setHour(e){skyIndex=Math.max(0,Math.min(e,23))}skyTime=0,skyIndex=12,tick=e=>{for(skyTime+=e;skyTime>=intervals[skyIndex];)skyTime-=intervals[skyIndex],skyIndex++,skyIndex>=intervals.length-1&&(skyIndex=0,skyTime=0);let t=skyTime/intervals[skyIndex],i=inclinationList[skyIndex],n=i+(inclinationList[skyIndex+1]-i)*t;api.getPlayerIds().forEach((e=>{if(api.setClientOption(e,"skyBox",{type:"earth",inclination:n,turbidity:1,luminance:.75,azimuth:0,infiniteDistance:3,vertexTint:[255,255,255]}),skyIndex>23||skyIndex<4){if(Math.random()>.95){let t=Math.floor(120*Math.random()-60),i=Math.floor(120*Math.random()-60),n=api.getPosition(e),a=n[0]+t,s=n[2]+i;spawnMob(Math.random()>1/3?nightMobs[0]:nightMobs[2],a,s)}}else{for(const e of spawnedMobs)e&&api.checkValid(e)&&api.killLifeform(e);spawnedMobs=[]}}))};let recipesForClock=[{requires:[{items:["Gold Bar"],amt:2}],produces:1}];function onPlayerAltAction(e,t,i,n,a){if(null!=api.getHeldItem(e)||"Air"!=a)if(a.includes("Bed")&&"Bedrock"!=a)if(skyIndex>=23||skyIndex<5){if(sleeping.includes(e)?(api.removeEffect(e,"Frozen"),delete sleeping[sleeping.indexOf(e)]):(api.sendMessage(e,"Sleeping..."),sleeping.push(e),api.applyEffect(e,"Frozen",null,{displayName:"Sleeping",icon:"Red Bed"})),sleeping.length>=api.getNumPlayers()*sleepRate){api.broadcastMessage("Sleeping through this night..."),skyIndex=6;for(const e of sleeping)api.checkValid(e)&&null!=e&&api.removeEffect(e,"Frozen");sleeping=[]}}else api.sendMessage(e,"You can sleep only at night.");else"Yellow Sticky Paintball Explosive Item"==api.getHeldItem(e)?.name&&api.sendMessage(e,"Time: "+skyIndex,{color:"yellow"})}function onPlayerAttemptCraft(e,t,i){let n=!0;try{onPlayerAttemptCraft1}catch(e){n=!1}if(n)var a=onPlayerAttemptCraft1(e,t,i);if("Yellow Sticky Paintball Explosive Item"==t){if(!api.hasItem(e,"Yellow Sticky Paintball Explosive Item")){let t=[{str:api.getEntityName(e)+" has made the advancement "},{str:"[Who needs that?]",style:{color:"lime"}}];api.broadcastMessage(t)}return api.giveItem(e,"Yellow Sticky Paintball Explosive Item",null,{customDisplayName:"Clock",customDescription:"Right click to get time."}),api.removeItemName(e,"Gold Bar",2),api.removeItemName(e,"Iron Bar",3),"preventCraft"}return a}function onPlayerJoin(e){recipesForClock=[{requires:[{items:["Gold Bar"],amt:2},{items:["Iron Bar"],amt:3}],produces:1}],api.editItemCraftingRecipes(e,"Yellow Sticky Paintball Explosive Item",recipesForClock);let t=!0;try{onPlayerJoin1}catch(e){t=!1}t&&onPlayerJoin1(e)}
