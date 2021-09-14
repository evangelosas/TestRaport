<script lang="ts">
  import {
    Scene,
    PerspectiveCamera,
    WebGLRenderer,
    DirectionalLight,
  } from "three";
  import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader.js";

  const scene = new Scene();
  const camera = new PerspectiveCamera(75, innerWidth / innerHeight, 0.1, 100);
  camera.position.z = 0.5;
  const loader = new GLTFLoader();
  const light = new DirectionalLight(0xffffff, 1);
  light.position.set(0, 0, 1);

  loader.load(
    "./Megan.glb",
    function (gltf) {
      console.log("Model was loaded successfully.");
      console.log(gltf);
      scene.add(gltf.scene);
      animate();

    },
    function (xhr) {
      console.log((xhr.loaded / xhr.total) * 100 + "% loaded");
    },
    function (error) {
      console.log("There was an error when tried to load the model.");
    }
  );
  
  scene.add(light);
  const renderer = new WebGLRenderer({antialias:true});
  renderer.setSize(innerWidth, innerHeight);
  document.body.appendChild(renderer.domElement);
  
  function animate() {
    requestAnimationFrame(animate);
    renderer.render(scene, camera);
  }
  


</script>
