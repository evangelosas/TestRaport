<script>
  import { animationsList, numberOfAnimations } from "./AnimationStore.js";
  import GuiForAnimation from "./GuiForAnimation.svelte";
  import EventRecorder from "./EventRecorder.svelte";
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
  import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";
  export let file;
  let clock = new Clock();
  let renderer, scene, camera, mixer, action;
  let animations;
  let activeAnimation = {
    index: -1,
    name: "",
  };

  init();
  render();

  function init() {
    scene = new Scene();
    scene.background = new Color(0xf1f1f1);

    camera = new PerspectiveCamera(75, innerWidth / innerHeight, 0.1, 100);
    camera.position.z = 0.5;

    const frontLight = new DirectionalLight(0xffffff, 1);
    frontLight.position.set(0, 0, 1);
    scene.add(frontLight);

    const backLight = new DirectionalLight(0x888888, 1);
    backLight.position.set(0, 0, -1);
    scene.add(backLight);

    const leftlight = new DirectionalLight(0x330000, 1);
    leftlight.position.set(-2, 0, 0);
    leftlight.rotation.set(0, 270, 0);
    scene.add(leftlight);

    const rightLight = new DirectionalLight(0x003300, 1);
    rightLight.position.set(2, 0, 0);
    rightLight.rotation.set(0, 90, 0);
    scene.add(rightLight);

    const loader = new GLTFLoader();
    loader.load(
      file,
      function (gltf) {
        console.log("Model was loaded successfully.");
        scene.add(gltf.scene);
        animations = gltf.animations;
        mapAnimationNames(animations);
        mixer = new AnimationMixer(gltf.scene);
        playAnimation(0);
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

    const controls = new OrbitControls(camera, renderer.domElement);
    controls.enablePan = false;
    controls.enableZoom = false;
    controls.update();
  }

  function render() {
    const dt = clock.getDelta();
    if (mixer) mixer.update(dt);
    requestAnimationFrame(render);
    renderer.render(scene, camera);
  }

  function mapAnimationNames(unmappedAnimations) {
    for (
      let animationIndex = 0;
      animationIndex < unmappedAnimations.length;
      ++animationIndex
    ) {
      $animationsList = [
        ...$animationsList,
        {
          id: animationIndex + 1,
          name: unmappedAnimations[animationIndex].name,
        },
      ];
    }
    numberOfAnimations.set(unmappedAnimations.length);
  }

  function processUpdateAnimationEvent(event) {
    playAnimation(event.detail.index - 1, event.detail.status);
  }

  function playAnimation(index, startPlaying = false) {
    if (!action) {
      action = mixer.clipAction(animations[index]);
    }
    if (activeAnimation.index != index) {
      mixer.stopAllAction();
    }
    if (startPlaying) {
      console.log("Start animation " + animations[index].name);
      action.play();
      activeAnimation.name = animations[index].name;
      activeAnimation.index = index;
    } else {
      console.log("Stop animation " + animations[index].name);
      action.stop();
      activeAnimation.name = "";
      activeAnimation.index = index;
    }
  }
</script>

<GuiForAnimation on:updateAnimation={processUpdateAnimationEvent} />
<EventRecorder
  url="ws://localhost:8080"
  on:updateAnimation={processUpdateAnimationEvent}
/>

{#if activeAnimation.name != ""}
  <h2>
    Now playing {activeAnimation.name}
  </h2>
{/if}

<style>
  h2 {
    position: absolute;
    bottom: 80px;
    left: 80px;
    color: red;
  }
</style>
