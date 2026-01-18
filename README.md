<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Ferrari Endless Road</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <style>
    html, body {
      margin: 0;
      padding: 0;
      overflow: hidden;
      font-family: Arial, sans-serif;
      background: #000;
    }
    #menu {
      position: absolute;
      inset: 0;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      background: radial-gradient(circle at top, #222, #000);
      color: white;
      z-index: 10;
    }
    #menu h1 {
      font-size: 48px;
      margin-bottom: 20px;
    }
    .btn {
      padding: 15px 40px;
      margin: 10px;
      font-size: 20px;
      cursor: pointer;
      border: none;
      border-radius: 30px;
      background: red;
      color: white;
    }
    #hud {
      position: absolute;
      top: 10px;
      left: 10px;
      color: white;
      font-size: 18px;
      display: none;
    }
  </style>
</head>
<body>

<div id="menu">
  <h1>Ferrari Unlimited Road</h1>
  <button class="btn" onclick="startGame()">Start</button>
  <button class="btn" onclick="alert('Settings coming soon')">Settings</button>
</div>

<div id="hud">Speed: <span id="speed">0</span> km/h</div>

<script src="https://cdn.jsdelivr.net/npm/three@0.158.0/build/three.min.js"></script>
<script>
  let scene, camera, renderer;
  let road, ferrari;
  let speed = 0;
  const maxSpeed = 545;
  let playing = false;

  function init() {
    scene = new THREE.Scene();

    camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    camera.position.set(0, 5, 10);

    renderer = new THREE.WebGLRenderer({ antialias: true });
    renderer.setSize(window.innerWidth, window.innerHeight);
    document.body.appendChild(renderer.domElement);

    const light = new THREE.DirectionalLight(0xffffff, 1);
    light.position.set(5, 10, 5);
    scene.add(light);

    const ambient = new THREE.AmbientLight(0x404040);
    scene.add(ambient);

    // Road
    const roadGeo = new THREE.PlaneGeometry(20, 2000);
    const roadMat = new THREE.MeshStandardMaterial({ color: 0x333333 });
    road = new THREE.Mesh(roadGeo, roadMat);
    road.rotation.x = -Math.PI / 2;
    road.position.z = -900;
    scene.add(road);

    // Ferrari (simple placeholder car)
    const carGeo = new THREE.BoxGeometry(2, 1, 4);
    const carMat = new THREE.MeshStandardMaterial({ color: 0xff0000 });
    ferrari = new THREE.Mesh(carGeo, carMat);
    ferrari.position.y = 0.5;
    scene.add(ferrari);

    animate();
  }

  function startGame() {
    document.getElementById('menu').style.display = 'none';
    document.getElementById('hud').style.display = 'block';
    playing = true;
  }

  function animate() {
    requestAnimationFrame(animate);

    if (playing) {
      speed += 0.5;
      if (speed > maxSpeed) speed = maxSpeed;

      road.position.z += speed * 0.2;
      if (road.position.z > 0) road.position.z = -900;

      document.getElementById('speed').textContent = Math.floor(speed);
    }

    renderer.render(scene, camera);
  }

  window.addEventListener('resize', () => {
    camera.aspect = window.innerWidth / window.innerHeight;
    camera.updateProjectionMatrix();
    renderer.setSize(window.innerWidth, window.innerHeight);
  });

  init();
</script>

</body>
</html>
