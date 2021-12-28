<script>
  import * as THREE from "three";
  import * as SC from "svelte-cubed";
  import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";
  import { MapControls } from "three/examples/jsm/controls/OrbitControls.js";
  import { onMount } from "svelte";
  import { assets } from "$app/paths";

  const gltfLoader = new GLTFLoader();
  let modell;
  let mesh;

  $: duck = modell;
  $: duckmesh = mesh;

  onMount(async () => {
    gltfLoader.load(assets + "/models/Duck/glTF/Duck.gltf", (model) => {
      // model.scene.castShadow = true;
      console.log(model.scene);
      modell = model.scene;
      mesh = model.scene.children[0];
      console.log("suceess");
    });
  });

  let sw = 1;
  let sh = 1;
  let sd = 1;
  let sx = 1;
  let sy = 1;

  let tenderera = false;

  let spin = 0;

  SC.onFrame(() => {
    spin += 0.01;
  });
</script>

<div class="w-full h-full">
  <SC.Canvas antialias background="{new THREE.Color('papayawhip')}" shadows>
    <!-- <SC.Mesh
    geometry="{new THREE.BoxGeometry()}"
    material="{new THREE.MeshStandardMaterial({ color: 0xff3e00 })}"
    scale="{[sw, sh, sd]}"
    rotation="{[0, spin, 0]}"
    castShadow
  /> -->
    <!-- <SC.Mesh
    geometry="{duckmesh}"
    material="{new THREE.MeshStandardMaterial({ color: 0xff3e00 })}"
    scale="{[sw, sh, sd]}"
    rotation="{[0, spin, 0]}"
    castShadow
  /> -->
    <SC.Primitive
      object="{duck}"
      scale="{[0.5, 0.5, 0.5]}"
      position="{[0, -0.5, 0]}"
      rotation="{[0, spin, 0]}"
      castShadow="{true}"
    />
    <SC.PerspectiveCamera position="{[sx, sy, 3]}" />
    <SC.MapControls enableZoom="{false}" maxPolarAngle="{Math.PI * 0.51}" />
    <SC.AmbientLight intensity="{0.6}" />
    <SC.DirectionalLight
      intensity="{0.6}"
      position="{[-2, 3, 2]}"
      shadow="{{ mapSize: [2048, 2048] }}"
    />

    <SC.Group position="{[0, -sh / 2, 0]}">
      <SC.Mesh
        geometry="{new THREE.PlaneGeometry(50, 50)}"
        material="{new THREE.MeshStandardMaterial({ color: 'burlywood' })}"
        rotation="{[-Math.PI / 2, 0, 0]}"
        receiveShadow
      />

      <SC.Primitive
        object="{new THREE.GridHelper(50, 50, 0x444444, 0x555555)}"
        position="{[0, 0.001, 0]}"
      />
    </SC.Group>
  </SC.Canvas>

  <div class="absolute flex flex-col w-12">
    <label
      ><input
        type="range"
        bind:value="{sw}"
        min="{0.1}"
        max="{3}"
        step="{0.1}"
      />
      width</label
    >
    <label
      ><input
        type="range"
        bind:value="{sh}"
        min="{0.1}"
        max="{3}"
        step="{0.1}"
      />
      height</label
    >
    <label
      ><input
        type="range"
        bind:value="{sd}"
        min="{0.1}"
        max="{3}"
        step="{0.1}"
      />
      depth</label
    >
    <label
      ><input type="range" bind:value="{sx}" min="{0}" max="{100}" step="{1}" />
      x</label
    >
    <label
      ><input type="range" bind:value="{sy}" min="{0}" max="{200}" step="{1}" />
      y</label
    >
  </div>
</div>
