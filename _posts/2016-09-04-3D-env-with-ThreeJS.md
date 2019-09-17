---
layout: post
title:  "The Very First Prototyping With ThreeJS, PhysiJS and THREEx."
categories: [3D-Web]
tags: [ThreeJS]
read: 5 min
---

<p>Virtual Reality? All I ever wanted to build since was a virtual shopping mall. Turning around 360 degrees, glancing ambience made ups with partially real objects, a place where you can build your world! Let's get it started -- having a first impression rather than building one virtual world in too soon! I'm doing my very first prototyping with ThreeJS, PhysiJS and THREEx.</p> 

##### Some Brief Introduction

*   Three.js is a 3D library that tries to make it as easy as possible to get 3D content on a webpage. 
*   Physijs takes that philosophy to heart and makes physics simulations just as easy to run and is so easy for graphics newbies to get into 3D programming.
*   THREEx is games extensions for three.js.

##### References

*     https://threejs.org/
*     https://github.com/chandlerprall/Physijs
*     http://learningthreejs.com/data/THREEx/docs/THREEx.KeyboardState.html

### 3D Environment Prototyping

<iframe width="560" height="315" src="https://www.youtube.com/embed/c2_pkOd9n4A" frameborder="0" allowfullscreen></iframe>


#### Include ThreeJS And Some Main Three-scene-required Js

```js
    <script type="text/javascript" src="lib/js/3/Three.js"></script>

    <script type="text/javascript" src="lib/js/3/requestAnimFrame.js"></script>
    <script type="text/javascript" src="lib/js/3/Stats.js"></script>
    <script type="text/javascript" src="lib/js/3/Detector.js"></script>s

    <script type="text/javascript" src="lib/js/3/effects/Mirror.js"></script>
```

#### Include THREEx Game Extensions for Three.js
```js
    <script type="text/javascript" src="lib/js/3/THREEx.FullScreen.js"></script>
    <script type="text/javascript" src="lib/js/3/THREEx.KeyboardState.js"></script>
    <script type="text/javascript" src="lib/js/3/THREEx.WindowResize.js"></script>
    <script type="text/javascript" src="lib/js/3/THREEx.CubeCamera.js"></script>
    <script type="text/javascript" src="lib/js/3/Raycaster.js"></script>
    <script type="text/javascript" src="lib/js/3/TrackballControls.js"></script>
```
<blockquote>
<small>
So that these gives us extra control to interact with the Three's scene.
Eg: Move, Rotate, Click etc.
</small>
</blockquote>

#### Include Three's Loaders 
```js
    <script type="text/javascript" src="lib/js/3/loader/ColladaLoader.js"></script>
```
<blockquote>
<small>
We use the loaders to inlude 3D models files with .Dae extension. 
Of course there are others possible loaders from Three to be used (eg. OBJLoader2, MTLLoader2, ALLoader, JSONLoader). However in this case, we are using ColladaLoader for demonstration purpose :)
</small>
</blockquote>

#### Finally We Also Include Physics plugin for three.js
```js
    <script type="text/javascript" src="lib/js/physijs/physi.js"></script>
```
<blockquote>
    <small>Physijs takes that philosophy to heart and makes physics simulations just as easy to run.</small>
</blockquote>

#### Html Element for ThreeScene to Be Created.
```html
    <div id="ThreeJS" class="toucharea" ></div>
```

***

#### Scripting
```js
    $(document).ready(function(){
    var $touchArea = $('#ThreeJS'), 
                    touchStarted= false,
                    currX = 0,
                    currY = 0,
                    cachedX = 0,
                    cachedY = 0;

                var controls, stats;
                var keyboard = new THREEx.KeyboardState();
                var clock = new THREE.Clock();

                var cube;
                var mouse = new THREE.Vector2();   

                var directionVector = new THREE.Vector3();
    
                var clock = new THREE.Clock();
                var loader = new THREE.TextureLoader();
                
                //SCENE
                scene = new Physijs.Scene();
                scene.setGravity(new THREE.Vector3( 0, -30, 0 ));
                projector = new THREE.Projector();
                
                // EVENTS 
                THREEx.WindowResize(renderer, textureCamera);
                THREEx.FullScreen.bindKey({ charCode : 'm'.charCodeAt(0) });

                id = document.getElementById( 'ThreeJS' );

                // Initialize Loader
                var texture1=new THREE.Texture();
                var manager = new THREE.LoadingManager();
                var obj_loader= new THREE.OBJLoader(manager);

                THREE.DefaultLoadingManager.onProgress = function ( item, loaded, total ) {
                    console.log( item, loaded, total );
                };
    });
```

#### Add Camera To Scnene
```js
    //CAMERA
    var VIEW_ANGLE = 45, ASPECT = SCREEN_WIDTH / SCREEN_HEIGHT, NEAR = 0.1, FAR = 20000;
    camera = new THREE.PerspectiveCamera( 50, window.innerWidth / window.innerHeight, 0.1, 20000 );
    textureCamera = new THREE.PerspectiveCamera( VIEW_ANGLE, ASPECT, NEAR, FAR );

    renderer = new THREE.WebGLRenderer();
    renderer.setSize( window.innerWidth, window.innerHeight );
    renderer.setClearColor(0xEEEEEE,1.0);
    effect = new THREE.StereoEffect(renderer);
```

#### Add Control Mechanism
```js
    //Controls
    controls = new THREE.TrackballControls( textureCamera );
    controls.rotateSpeed = 0.5;
    controls.zoomSpeed = 0.2;
    controls.panSpeed = 0.2;
    controls.noZoom = false;
    controls.noPan = false;
    controls.staticMoving = true;
    controls.dynamicDampingFactor = 0.3;
```

#### Adding STATS To Monitor Performance
```js
    // STATS
    stats = new Stats();
    stats.domElement.style.position = 'absolute';
    stats.domElement.style.bottom = '0px';
    stats.domElement.style.zIndex = 100;
    id.appendChild( stats.domElement );
```

#### Add Enought Light Souce To The Scene
```js
    //Add Direction Light Source Object
    var directionalLight = new THREE.DirectionalLight(0xffffff,0.2);
    directionalLight.position.set( 50, 50, 50 ).normalize();
    var directionalLight2 = new THREE.DirectionalLight(0xffffff);
    directionalLight2.position.set( -50, 30, 50 ).normalize();
    var directionalLight3 = new THREE.DirectionalLight(0xffffff);
    directionalLight3.position.set( -500, 30, 500 ).normalize();
    var directionalLight4 = new THREE.DirectionalLight(0xffffff);
    directionalLight4.position.set( -500, 30, -1000 ).normalize();
    scene.add( directionalLight );
```

#### Create Ground 
```js
    //ground
    walls = new THREE.Object3D();
    var living_area = new THREE.Object3D();
    var groundGeo = new THREE.PlaneGeometry(10000, 10000);

    var groundGeo = new THREE.PlaneGeometry(10000, 10000);

    var floorTexture = new  THREE.MeshBasicMaterial( {color: 0xffffff,  map: loader.load('textures/walls/floor2.jpg',function(texture){
        texture.needsUpdate=true;
    }
    )});

    var ground = new THREE.Mesh(groundGeo, floorTexture);
        ground.overdraw = true;
        ground.position.set(0, -wallWidth, 0);
        ground.rotation.x = -Math.PI/2;
        walls.add(ground);
```

#### Sample Adding Ceiling Light to Living Area
```js
    //CeilingLight from tv number1 
    var ceilinglight = new THREE.PointLight( 0xff9d2a, 1, 488);
        ceilinglight.position.set(-850*5,-wallWidth+2000, 205*5 );
        ceilinglight.rotation.set(-Math.PI/2,0, Math.PI/2);
        ceilinglight.shadowCameraVisible = true;
        ceilinglight.shadowDarkness = 0.95;
        ceilinglight.intensity = 100;
        ceilinglight.castShadow = true;

    living_area.add(ceilinglight);
```

#### Add Wall 3D Object.Dae to Scene's Wall Element
```js
    /************************************************
    *           ColladaLoader For Dae               *
    ************************************************/

    var colladaLoader1 = new THREE.ColladaLoader();
        colladaLoader1.load('Dae/wall.dae', function (object) {
            object.scene.position.set(-1000*5,-wallWidth, -1000*5);
            object.scene.rotation.set(-Math.PI/2,0, -Math.PI/2);
            object.scene.scale.set(5, 5, 5);
            object.name="wall";
            walls.add( object.scene );
        });
``` 

#### Add A Moving Cube Represents Self (To Simulate self move, rotate)
```js
    // create an array with six textures for a cool cube
    var materialArray = [];
    materialArray.push(
        new THREE.MeshBasicMaterial( { map: THREE.ImageUtils.loadTexture( 'img/xpos.png' ) }));
    materialArray.push(
        new THREE.MeshBasicMaterial( { map: THREE.ImageUtils.loadTexture( 'img/xneg.png' ) }));
    materialArray.push(
        new THREE.MeshBasicMaterial( { map: THREE.ImageUtils.loadTexture( 'img/ypos.png' ) }));
    materialArray.push(
        new THREE.MeshBasicMaterial( { map: THREE.ImageUtils.loadTexture( 'img/yneg.png' ) }));
    materialArray.push(
        new THREE.MeshBasicMaterial( { map: THREE.ImageUtils.loadTexture( 'img/zpos.png' ) }));
    materialArray.push(
        new THREE.MeshBasicMaterial( { map: THREE.ImageUtils.loadTexture( 'img/zneg.png' ) }));
    var MovingCubeMat = new THREE.MeshFaceMaterial(materialArray);
    var MovingCubeGeom = new THREE.CubeGeometry( 2000, 2000, 2000, 1, 1, 1, materialArray );

    MovingCube = new THREE.Mesh( MovingCubeGeom, MovingCubeMat );
    MovingCube.position.set(3100, -2000, 2100);
    MovingCube.rotation.y = Math.PI/2;
    MovingCube.castShadow = true;

    scene.add( MovingCube );  
    scene.add(living_area);
    scene.add(walls);
    scene.simulate();
    renderer.shadowMap.Enabled = false;
    id.appendChild( renderer.domElement );
```

#### Simulate Scene
```js
    requestAnimationFrame( animate );
    scene.simulate();
```

#### Update Animation Moving
```js

                    scene.simulate(undefined,1);
                    var delta = clock.getDelta(); // seconds.
                    var moveDistance = 400 * delta; // 400 pixels per second
                    var rotateAngle = Math.PI / 6 * delta;   // pi/2 radians (45 degrees) per second
                    
                    // local transformations

                    // move forwards/backwards/left/right
                    if ( keyboard.pressed("W") ){
                        if(MovingCube.position.y< -2200 || MovingCube.position.y> -1800 ){
                            MovingCube.rotation.x = 0;
                            MovingCube.position.y=-2200;
                        }   
                        else
                            MovingCube.translateZ( -moveDistance*1.5 );
                    }
                    if ( keyboard.pressed("S") ){
                        if(MovingCube.position.y< -2200 || MovingCube.position.y> -1800 ){
                            MovingCube.rotation.x = 0;
                            MovingCube.position.y=-2200;
                        }
                        else
                            MovingCube.translateZ(  moveDistance*1.5 );
                    }
                    if ( keyboard.pressed("A") ){
                        if(MovingCube.position.y< -2200 || MovingCube.position.y> -1800 ){
                            MovingCube.rotation.x = 0;
                            MovingCube.position.y=-2200;
                        }
                        else
                            MovingCube.translateX( -moveDistance*1.5 );
                    }
                    if ( keyboard.pressed("D") ){
                        if(MovingCube.position.y< -2200 || MovingCube.position.y> -1800 ){
                            MovingCube.rotation.x = 0;
                            MovingCube.position.y=-2200;
                        }
                        else
                            MovingCube.translateX(  moveDistance*1.5 );   
                    }

                    // rotate left/right/up/down  velocity
                    var rotation_matrix = new THREE.Matrix4().identity();
                    
                    if ( keyboard.pressed("Z") )
                    {
                        MovingCube.position.set(0, -wallWidth/2, 0);
                        MovingCube.rotation.y = -Math.PI/2;
                        render();
                    }
                    
                    var relativeCameraOffset = new THREE.Vector3(0,0,1);
                    var cameraOffset =relativeCameraOffset.applyMatrix4(MovingCube.matrixWorld);
                    textureCamera.position.x = cameraOffset.x;
                    textureCamera.position.y = cameraOffset.y;
                    textureCamera.position.z = cameraOffset.z;
                    var relativeCameraLookOffset = new THREE.Vector3(0,0,0);
                    var cameraLookOffset = relativeCameraLookOffset.applyMatrix4( MovingCube.matrixWorld );
                    textureCamera.lookAt( cameraLookOffset );
                    stats.update();
```
