ShadowImpact
============

This is a simple shadow casting/ lighting plugin for  [ImpactJS](http://www.impactjs.com).

Features
--------

 * dynamic light sources for multiple entities
 * pulsating shine (torch style)
 * configurable colors including radial gradients
 * configurable light source offset
 * separate drawing methods for darkening layer and light layer 


Example
-------

To use the example, copy and overwrite the example files into an existing Jump'n'Run Demo directory
I provided only the Jump'n'Run Demo JS files without the media files or the impact framework.

For more Information about ImpactJS and the Jump'n'Run Demo see http://www.impactjs.com

Example Image
-------------

![](http://coldspace.henklein.com/v020_screenshot.png)

Installation
------------
* copy the plugins folder into your lib folder
* register the plugin in your main.js:

```
.requires(
	'plugins.lights'
)
```

* register a LightManager instance in your main.js:

```
MyGame = ig.Game.extend({
	lightManager: '',
	init: function() {
	this.lightManager = new ig.LightManager();
}
```

* add the light manager to the update cycle (in your main.js):

```
update: function() {
	this.parent();
	this.lightManager.update();
}
```

* add the Lighting Draw calls to the impact.js lib/impact/game.js around the entitydrawing ( or somewhere else )

```
draw: function() {
	...
	this.lightManager.drawLightMap();
	this.drawEntities();
 	this.lightManager.drawShadowMap();
}
```

* add your light sources


Usage
-----

The Lighting Engine is Entity driven. That means, you're attaching a light source to an entity.
Lets add a light to our Player:

* in your init function, you add the light source:

```
init: function(x,y,settings) {
...
	this.light = ig.game.lightManager.addLight(this, {
			angle: 5,	
			angleSpread: 125,	
			radius: 80,			
			color:'rgba(255,255,255,0.1)',	// there is an extra shadowColor option
			useGradients: true,	// false will use color/ shadowColor
			shadowGradientStart: 'rgba(0,0,0,0.1)',			// 2-stop radial gradient at 0.0 and 1.0
			shadowGradientStop: 'rgba(0,0,0,0.1)',
			lightGradientStart: 'rgba(255,255,100,0.1)',	// 2-stop radial gradient at 0.0 and 1.0
			lightGradientStop: 'rgba(0,0,0,0.6)',
			pulseFactor: 5,
			lightOffset: {x:0,y:0}		// lightsource offset from the middle of the entity
		});

}
```
* to kill a light, you can use the ```removeLight(light)``` and ```removeLightByIndex(index)``` method.
* that's it! 
* you can change any configuration attribute within game livecycle.
  for example, if your player is in a flip-state, the starting angle has to flip also:

```
update: function() {
	...
	if (this.flip) {
		this.light.angle = 185;
	} else {
		this.light.angle = 5;
	}
}
```

Changelog
---------

v0.20
* improved canvas drawing
* added gradients
* provided example within the repo

v0.11
* added removeLight method

v0.10
* initial commit
