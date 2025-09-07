<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Kerala Web3 Scene</title>
<style>
  body { margin:0; overflow:hidden; background: #0d0d0d; }
  canvas { display:block; }
</style>
</head>
<body>
<script src="https://cdn.jsdelivr.net/npm/three@0.157.0/build/three.min.js"></script>
<script>
// Scene, Camera, Renderer
const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(60, window.innerWidth/window.innerHeight, 0.1, 1000);
const renderer = new THREE.WebGLRenderer({antialias:true});
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);

// Lighting
const light = new THREE.PointLight(0xffffff, 1.5);
light.position.set(50,50,50);
scene.add(light);

// Palm Tree (Cylinder + Sphere leaves)
const trunkGeometry = new THREE.CylinderGeometry(0.5, 0.5, 10, 16);
const trunkMaterial = new THREE.MeshStandardMaterial({color:0x8B4513});
const trunk = new THREE.Mesh(trunkGeometry, trunkMaterial);
trunk.position.y = 5;
scene.add(trunk);

for(let i=0;i<5;i++){
  const leafGeometry = new THREE.SphereGeometry(2, 16, 16);
  const leafMaterial = new THREE.MeshStandardMaterial({color:0x22D3EE});
  const leaf = new THREE.Mesh(leafGeometry, leafMaterial);
  leaf.position.set(Math.random()*4-2, 10+Math.random()*2, Math.random()*4-2);
  scene.add(leaf);
}

// Floating ETH coins
const coinGeometry = new THREE.TorusGeometry(0.3,0.1,16,100);
const coinMaterial = new THREE.MeshStandardMaterial({color:0xFEE715});
const coins = [];
for(let i=0;i<20;i++){
  const coin = new THREE.Mesh(coinGeometry, coinMaterial);
  coin.position.set(Math.random()*20-10, Math.random()*15, Math.random()*20-10);
  scene.add(coin);
  coins.push(coin);
}

camera.position.z = 25;

// Animate scene
function animate(){
  requestAnimationFrame(animate);
  coins.forEach(c => { 
    c.position.y -= 0.05; 
    if(c.position.y < 0) c.position.y = 15; 
    c.rotation.x += 0.02; 
    c.rotation.y += 0.02; 
  });
  renderer.render(scene, camera);
}
animate();

// Handle resize
window.addEventListener('resize', ()=>{
  camera.aspect = window.innerWidth/window.innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(window.innerWidth, window.innerHeight);
});
</script>
</body>
</html>
