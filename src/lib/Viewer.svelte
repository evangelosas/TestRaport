<script lang="ts">
  import {
    Scene,
    PerspectiveCamera,
    WebGLRenderer,
    DirectionalLight,
    AnimationMixer,
    Color,
    Clock,
  } from "three";
  import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader.js";

  let clock = new Clock();
  let renderer, scene, camera, mixer;

  init();
  render();
  
  function init() {
    scene = new Scene();
    scene.background = new Color(0xf1f1f1);

    camera = new PerspectiveCamera(75, innerWidth / innerHeight, 0.1, 100);
    camera.position.z = 0.5;

    const light = new DirectionalLight(0xffffff, 1);
    light.position.set(0, 0, 1);
    scene.add(light);

    const loader = new GLTFLoader();
    loader.load(
      "./Megan.glb",
      function (gltf) {
        console.log("Model was loaded successfully.");
        console.log(gltf);
        scene.add(gltf.scene);
        playAnimations(gltf.scene, gltf.animations);
      },
      function (workload) {
        console.log((workload.loaded / workload.total) * 100 + "% loaded");
      },
      function () {
        console.log("There was an error when tried to load the model.");
      }
    );

    renderer = new WebGLRenderer({ antialias: true });
    renderer.setSize(innerWidth, innerHeight);
    document.body.appendChild(renderer.domElement);
  }

  function render() {
    const dt = clock.getDelta();
    if (mixer) mixer.update(dt);
    requestAnimationFrame(render);
    renderer.render(scene, camera);
  }

  function playAnimations(model, animations) {
      mixer = new AnimationMixer(model);
      const clip = animations[0];
      const action = mixer.clipAction(clip);
      action.play();
    }
</script>
