<script>
  import * as THREE from "three";
  import * as SC from "svelte-cubed";
  import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";
  import { MapControls } from "three/examples/jsm/controls/OrbitControls.js";
  import { onMount } from "svelte";
  import { assets } from "$app/paths";
  import { InteractionManager } from "three.interactive";

  const gltfLoader = new GLTFLoader();
  let modell;
  let mesh;
  let dada;
  let oby;

  $: duck = modell;
  $: duckmesh = mesh;
  let interactionManager;

  function moveCamera(v3) {
    sx = v3.x + 1;
    sy = v3.y + 1;
    sz = 3;
  }
  onMount(async () => {
    gltfLoader.load(assets + "/models/Duck/glTF/Duck.gltf", (model) => {
      // model.scene.castShadow = true;
      console.log(model.scene);
      modell = model.scene;
      mesh = model.scene.children[0];
      console.log(dada.root.camera, dada.self);
      console.log(
        new THREE.PerspectiveCamera(
          45,
          window.innerWidth / window.innerHeight,
          0.1,
          1000
        )
      );
      interactionManager = new InteractionManager(
        dada.root.renderer,
        dada.root.camera.object,
        dada.root.canvas
      );

      interactionManager.add(dada.self);

      dada.self.addEventListener("click", (event) => {
        console.log(dada.self.position);
        dada.root.camera.object.lookAt(dada.self.position);
        //moveCamera(dada.self.position);
      });
      console.log("suceess");
    });
  });

  let sw = 1;
  let sh = 1;
  let sd = 1;
  let sx = 1;
  let sy = 1;
  let sz = 3;

  let tenderera = false;

  let spin = 0;

  SC.onFrame(() => {
    spin += 0.01;
    if (interactionManager) {
      interactionManager.update();
    }
  });
</script>

<div class="w-full h-full">
  <SC.Canvas antialias background={new THREE.Color("papayawhip")} shadows>
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
      bind:this={dada}
      object={duck}
      scale={[0.5, 0.5, 0.5]}
      position={[0, -0.5, 0]}
      rotation={[0, spin, 0]}
      castShadow={true}
    />
    <SC.PerspectiveCamera position={[sx, sy, sz]} />
    <SC.OrbitControls enableZoom={false} maxPolarAngle={Math.PI * 0.51} />
    <SC.AmbientLight intensity={0.6} />
    <SC.DirectionalLight
      intensity={0.6}
      position={[-2, 3, 2]}
      shadow={{ mapSize: [2048, 2048] }}
    />

    <SC.Group position={[0, -sh / 2, 0]}>
      <SC.Mesh
        geometry={new THREE.PlaneGeometry(50, 50)}
        material={new THREE.MeshStandardMaterial({ color: "burlywood" })}
        rotation={[-Math.PI / 2, 0, 0]}
        receiveShadow
      />

      <SC.Primitive
        object={new THREE.GridHelper(50, 50, 0x444444, 0x555555)}
        position={[0, 0.001, 0]}
      />
    </SC.Group>
  </SC.Canvas>

  <div class="absolute flex flex-col w-12">
    <label
      ><input type="range" bind:value={sw} min={0.1} max={3} step={0.1} />
      width</label
    >
    <label
      ><input type="range" bind:value={sh} min={0.1} max={3} step={0.1} />
      height</label
    >
    <label
      ><input type="range" bind:value={sd} min={0.1} max={3} step={0.1} />
      depth</label
    >
    <label
      ><input type="range" bind:value={sx} min={0} max={100} step={1} />
      x</label
    >
    <label
      ><input type="range" bind:value={sy} min={0} max={200} step={1} />
      y</label
    >
  </div>
</div>
