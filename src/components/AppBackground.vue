<template>
  <div class="app-background">
    <!-- <div class="blob" v-for="blob in blobs" :style="blob">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        version="1.1"
        xmlns:xlink="http://www.w3.org/1999/xlink"
        viewBox="0 0 200 200"
        :width="blob.width"
        :height="blob.height"
      >
        <path
          transform="matrix(1 0 0 1 100 100)"
          fill="var(--color-primary)"
          opacity=".25"
        >
          <animate
            repeatCount="indefinite"
            attributeName="d"
            :dur="Math.random() * 5 + 5"
            :values="blob.paths.join('; ')"
            keySplines="
            0.42, 0.0, 0.58, 1.0;
            0.42, 0.0, 0.58, 1.0;
            0.42, 0.0, 0.58, 1.0;
            0.42, 0.0, 0.58, 1.0;
            0.42, 0.0, 0.58, 1.0;"
            keyTimes="
			0;0.33;0.55;0.66;0.88;1"
            calcMode="spline"
          ></animate>
        </path>
      </svg>
    </div> -->
  </div>
</template>

<script setup>
import { random } from "@/utils/Algebra";
import { pickRandom } from "@/utils/Array";

const paths = [
  "M27.6,-33.9C40.2,-28.9,57.7,-26.7,64.5,-18C71.4,-9.4,67.6,5.7,60.4,17.2C53.3,28.7,42.9,36.8,32.3,40.6C21.7,44.5,10.8,44.2,-2.4,47.6C-15.7,50.9,-31.4,57.8,-41.5,53.8C-51.7,49.8,-56.3,34.8,-62.9,19.3C-69.5,3.8,-78,-12.1,-77.4,-28.6C-76.8,-45,-67,-61.9,-52.6,-66.2C-38.1,-70.5,-19.1,-62.2,-5.8,-54.3C7.6,-46.4,15.1,-38.8,27.6,-33.9Z",
  "M47.6,-67.2C57.9,-58.1,59.8,-39.1,64.2,-21.8C68.5,-4.5,75.2,11,71.4,23.3C67.5,35.6,53.2,44.5,39.5,53.3C25.9,62.1,12.9,70.7,2.1,67.9C-8.8,65.1,-17.6,50.8,-29.1,41.2C-40.6,31.7,-54.7,27,-59.7,17.9C-64.8,8.7,-60.8,-4.9,-55.5,-17.1C-50.2,-29.3,-43.7,-40.2,-34.2,-49.6C-24.7,-59,-12.4,-67,3.2,-71.3C18.7,-75.7,37.3,-76.4,47.6,-67.2Z",
  "M35,-51.9C45.5,-40.5,54.4,-30.5,63.5,-17.2C72.6,-3.8,81.9,12.9,78.2,25.8C74.5,38.6,57.9,47.7,42.7,51.7C27.5,55.8,13.7,54.8,-1.5,56.9C-16.8,59,-33.5,64.1,-48.4,59.9C-63.3,55.8,-76.4,42.4,-82.7,26.2C-89,9.9,-88.4,-9.4,-81.3,-24.9C-74.2,-40.4,-60.4,-52.3,-45.7,-62.4C-31.1,-72.5,-15.5,-80.8,-1.7,-78.5C12.2,-76.2,24.4,-63.3,35,-51.9Z",
  "M41.1,-55.1C53.3,-47.7,63.4,-35.8,69.8,-21.4C76.1,-7.1,78.9,9.7,71.6,20.5C64.3,31.3,47.1,36.2,33.5,44.6C19.9,53,9.9,64.9,-2.7,68.6C-15.3,72.3,-30.6,67.7,-40.7,58.2C-50.8,48.7,-55.8,34.2,-61.1,19.5C-66.4,4.8,-72,-10.2,-67.2,-20.8C-62.3,-31.4,-47,-37.5,-34.1,-44.7C-21.1,-51.9,-10.6,-60.2,1.9,-62.8C14.4,-65.5,28.9,-62.6,41.1,-55.1Z",
  "M36.8,-47.6C46.2,-43.7,51.5,-31.1,60,-16.7C68.5,-2.4,80.1,13.7,75.8,23.9C71.5,34,51.2,38.3,35.9,45.2C20.6,52.2,10.3,61.8,-1.3,63.6C-12.9,65.4,-25.7,59.2,-40.5,52.1C-55.2,45,-71.9,36.9,-77.6,24.1C-83.3,11.3,-78.1,-6.2,-71.9,-22.8C-65.8,-39.4,-58.6,-55.1,-46.4,-58C-34.3,-61,-17.1,-51.2,-1.7,-48.8C13.6,-46.4,27.3,-51.4,36.8,-47.6Z",
  "M27.6,-33.9C40.2,-28.9,57.7,-26.7,64.5,-18C71.4,-9.4,67.6,5.7,60.4,17.2C53.3,28.7,42.9,36.8,32.3,40.6C21.7,44.5,10.8,44.2,-2.4,47.6C-15.7,50.9,-31.4,57.8,-41.5,53.8C-51.7,49.8,-56.3,34.8,-62.9,19.3C-69.5,3.8,-78,-12.1,-77.4,-28.6C-76.8,-45,-67,-61.9,-52.6,-66.2C-38.1,-70.5,-19.1,-62.2,-5.8,-54.3C7.6,-46.4,15.1,-38.8,27.6,-33.9Z",
  "M32.5,-47C37.4,-41.2,33.4,-25.4,37.7,-12.2C42.1,0.9,54.7,11.4,53.8,18.5C52.9,25.6,38.5,29.4,27.3,40.7C16,52,8,70.9,1.9,68.2C-4.2,65.6,-8.3,41.4,-19.9,30.2C-31.4,19,-50.4,20.7,-56.3,15.3C-62.2,9.8,-55.1,-2.9,-46.8,-11.1C-38.4,-19.3,-28.9,-23,-20.9,-27.8C-12.9,-32.6,-6.5,-38.5,3.7,-43.5C13.8,-48.6,27.6,-52.8,32.5,-47Z",
  "M17.1,-23.3C21.3,-20.5,23.2,-14.3,25.5,-8.1C27.7,-1.9,30.2,4.4,35,18.1C39.8,31.9,47,53.2,41.5,59.3C36,65.4,18,56.4,0.7,55.5C-16.6,54.5,-33.2,61.5,-36,54.5C-38.7,47.5,-27.6,26.4,-34.8,10C-42,-6.5,-67.4,-18.3,-74,-32.1C-80.6,-45.8,-68.3,-61.4,-52.8,-60.5C-37.3,-59.6,-18.6,-42.2,-6.1,-33.8C6.4,-25.5,12.9,-26,17.1,-23.3Z",
  "M42.2,-64.1C45.5,-55.6,32.7,-31,37.8,-12.9C42.9,5.3,66,17.1,65.6,22.3C65.3,27.5,41.6,26,26.7,26.2C11.9,26.4,6,28.3,-3.1,32.5C-12.1,36.8,-24.3,43.5,-32.9,41.2C-41.6,39,-46.8,28,-47.2,17.5C-47.6,7,-43.2,-2.8,-37.3,-9.4C-31.4,-16,-24,-19.4,-17.5,-26.8C-11,-34.2,-5.5,-45.8,7,-55.3C19.4,-64.9,38.9,-72.6,42.2,-64.1Z",
  "M27.4,-35.7C41.3,-27.5,62.4,-27.3,62.2,-21.7C62,-16.1,40.4,-5,33,8.5C25.6,22,32.4,38,29.3,46.5C26.1,55.1,13.1,56.2,5.2,49C-2.6,41.8,-5.2,26.3,-12.5,19.1C-19.9,11.9,-31.9,13.1,-36.6,9.3C-41.4,5.6,-38.9,-3.1,-32.9,-7.3C-26.9,-11.6,-17.5,-11.3,-11.4,-22.1C-5.4,-32.8,-2.7,-54.6,2,-57.4C6.7,-60.2,13.4,-43.9,27.4,-35.7Z",
  "M43,-53.3C57,-49,70.6,-38.2,72.1,-25.4C73.5,-12.6,62.9,2.2,54.9,15.1C46.9,27.9,41.5,38.9,32.8,48.5C24.1,58.2,12.1,66.6,0.2,66.4C-11.7,66.1,-23.4,57.2,-27.1,45.9C-30.7,34.6,-26.4,20.9,-33.9,8.5C-41.4,-3.9,-60.9,-15,-64.4,-26.2C-68,-37.3,-55.7,-48.6,-42.3,-53.1C-29,-57.6,-14.5,-55.3,0,-55.3C14.5,-55.3,29,-57.6,43,-53.3Z",
];
const blobs = ref([]);

onMounted(() => {
  for (let i = 0; i < random(4, 10); i++) {
    let size = random(100, 400);
    let firstBlob = pickRandom(paths);
    let blob = {
      top: [random(-size / 2, window.innerHeight - size / 2), "px"].join(""),
      left: [random(-size / 2, window.innerWidth - size / 2), "px"].join(""),
      width: [size, "px"].join(""),
      height: [size, "px"].join(""),
      paths: [
        firstBlob,
        pickRandom(paths),
        pickRandom(paths),
        pickRandom(paths),
        pickRandom(paths),
        firstBlob,
      ],
    };
    blobs.value.push(blob);
  }
});
// const blobs = ref([
//   {
//     top: "50px",
//     left: "-20px",
//     width: "300px",
//     height: "300px",
//     paths: [
//       "M27.6,-33.9C40.2,-28.9,57.7,-26.7,64.5,-18C71.4,-9.4,67.6,5.7,60.4,17.2C53.3,28.7,42.9,36.8,32.3,40.6C21.7,44.5,10.8,44.2,-2.4,47.6C-15.7,50.9,-31.4,57.8,-41.5,53.8C-51.7,49.8,-56.3,34.8,-62.9,19.3C-69.5,3.8,-78,-12.1,-77.4,-28.6C-76.8,-45,-67,-61.9,-52.6,-66.2C-38.1,-70.5,-19.1,-62.2,-5.8,-54.3C7.6,-46.4,15.1,-38.8,27.6,-33.9Z",
//       "M47.6,-67.2C57.9,-58.1,59.8,-39.1,64.2,-21.8C68.5,-4.5,75.2,11,71.4,23.3C67.5,35.6,53.2,44.5,39.5,53.3C25.9,62.1,12.9,70.7,2.1,67.9C-8.8,65.1,-17.6,50.8,-29.1,41.2C-40.6,31.7,-54.7,27,-59.7,17.9C-64.8,8.7,-60.8,-4.9,-55.5,-17.1C-50.2,-29.3,-43.7,-40.2,-34.2,-49.6C-24.7,-59,-12.4,-67,3.2,-71.3C18.7,-75.7,37.3,-76.4,47.6,-67.2Z",
//       "M35,-51.9C45.5,-40.5,54.4,-30.5,63.5,-17.2C72.6,-3.8,81.9,12.9,78.2,25.8C74.5,38.6,57.9,47.7,42.7,51.7C27.5,55.8,13.7,54.8,-1.5,56.9C-16.8,59,-33.5,64.1,-48.4,59.9C-63.3,55.8,-76.4,42.4,-82.7,26.2C-89,9.9,-88.4,-9.4,-81.3,-24.9C-74.2,-40.4,-60.4,-52.3,-45.7,-62.4C-31.1,-72.5,-15.5,-80.8,-1.7,-78.5C12.2,-76.2,24.4,-63.3,35,-51.9Z",
//       "M41.1,-55.1C53.3,-47.7,63.4,-35.8,69.8,-21.4C76.1,-7.1,78.9,9.7,71.6,20.5C64.3,31.3,47.1,36.2,33.5,44.6C19.9,53,9.9,64.9,-2.7,68.6C-15.3,72.3,-30.6,67.7,-40.7,58.2C-50.8,48.7,-55.8,34.2,-61.1,19.5C-66.4,4.8,-72,-10.2,-67.2,-20.8C-62.3,-31.4,-47,-37.5,-34.1,-44.7C-21.1,-51.9,-10.6,-60.2,1.9,-62.8C14.4,-65.5,28.9,-62.6,41.1,-55.1Z",
//       "M36.8,-47.6C46.2,-43.7,51.5,-31.1,60,-16.7C68.5,-2.4,80.1,13.7,75.8,23.9C71.5,34,51.2,38.3,35.9,45.2C20.6,52.2,10.3,61.8,-1.3,63.6C-12.9,65.4,-25.7,59.2,-40.5,52.1C-55.2,45,-71.9,36.9,-77.6,24.1C-83.3,11.3,-78.1,-6.2,-71.9,-22.8C-65.8,-39.4,-58.6,-55.1,-46.4,-58C-34.3,-61,-17.1,-51.2,-1.7,-48.8C13.6,-46.4,27.3,-51.4,36.8,-47.6Z",
//       "M27.6,-33.9C40.2,-28.9,57.7,-26.7,64.5,-18C71.4,-9.4,67.6,5.7,60.4,17.2C53.3,28.7,42.9,36.8,32.3,40.6C21.7,44.5,10.8,44.2,-2.4,47.6C-15.7,50.9,-31.4,57.8,-41.5,53.8C-51.7,49.8,-56.3,34.8,-62.9,19.3C-69.5,3.8,-78,-12.1,-77.4,-28.6C-76.8,-45,-67,-61.9,-52.6,-66.2C-38.1,-70.5,-19.1,-62.2,-5.8,-54.3C7.6,-46.4,15.1,-38.8,27.6,-33.9Z",
//     ],
//   },
//   {
//     top: "200px",
//     left: "200px",
//     width: "200px",
//     height: "300px",
//     paths: [
//       "M32.5,-47C37.4,-41.2,33.4,-25.4,37.7,-12.2C42.1,0.9,54.7,11.4,53.8,18.5C52.9,25.6,38.5,29.4,27.3,40.7C16,52,8,70.9,1.9,68.2C-4.2,65.6,-8.3,41.4,-19.9,30.2C-31.4,19,-50.4,20.7,-56.3,15.3C-62.2,9.8,-55.1,-2.9,-46.8,-11.1C-38.4,-19.3,-28.9,-23,-20.9,-27.8C-12.9,-32.6,-6.5,-38.5,3.7,-43.5C13.8,-48.6,27.6,-52.8,32.5,-47Z",
//       "M17.1,-23.3C21.3,-20.5,23.2,-14.3,25.5,-8.1C27.7,-1.9,30.2,4.4,35,18.1C39.8,31.9,47,53.2,41.5,59.3C36,65.4,18,56.4,0.7,55.5C-16.6,54.5,-33.2,61.5,-36,54.5C-38.7,47.5,-27.6,26.4,-34.8,10C-42,-6.5,-67.4,-18.3,-74,-32.1C-80.6,-45.8,-68.3,-61.4,-52.8,-60.5C-37.3,-59.6,-18.6,-42.2,-6.1,-33.8C6.4,-25.5,12.9,-26,17.1,-23.3Z",
//       "M42.2,-64.1C45.5,-55.6,32.7,-31,37.8,-12.9C42.9,5.3,66,17.1,65.6,22.3C65.3,27.5,41.6,26,26.7,26.2C11.9,26.4,6,28.3,-3.1,32.5C-12.1,36.8,-24.3,43.5,-32.9,41.2C-41.6,39,-46.8,28,-47.2,17.5C-47.6,7,-43.2,-2.8,-37.3,-9.4C-31.4,-16,-24,-19.4,-17.5,-26.8C-11,-34.2,-5.5,-45.8,7,-55.3C19.4,-64.9,38.9,-72.6,42.2,-64.1Z",
//       "M27.4,-35.7C41.3,-27.5,62.4,-27.3,62.2,-21.7C62,-16.1,40.4,-5,33,8.5C25.6,22,32.4,38,29.3,46.5C26.1,55.1,13.1,56.2,5.2,49C-2.6,41.8,-5.2,26.3,-12.5,19.1C-19.9,11.9,-31.9,13.1,-36.6,9.3C-41.4,5.6,-38.9,-3.1,-32.9,-7.3C-26.9,-11.6,-17.5,-11.3,-11.4,-22.1C-5.4,-32.8,-2.7,-54.6,2,-57.4C6.7,-60.2,13.4,-43.9,27.4,-35.7Z",
//       "M43,-53.3C57,-49,70.6,-38.2,72.1,-25.4C73.5,-12.6,62.9,2.2,54.9,15.1C46.9,27.9,41.5,38.9,32.8,48.5C24.1,58.2,12.1,66.6,0.2,66.4C-11.7,66.1,-23.4,57.2,-27.1,45.9C-30.7,34.6,-26.4,20.9,-33.9,8.5C-41.4,-3.9,-60.9,-15,-64.4,-26.2C-68,-37.3,-55.7,-48.6,-42.3,-53.1C-29,-57.6,-14.5,-55.3,0,-55.3C14.5,-55.3,29,-57.6,43,-53.3Z",
//       "M32.5,-47C37.4,-41.2,33.4,-25.4,37.7,-12.2C42.1,0.9,54.7,11.4,53.8,18.5C52.9,25.6,38.5,29.4,27.3,40.7C16,52,8,70.9,1.9,68.2C-4.2,65.6,-8.3,41.4,-19.9,30.2C-31.4,19,-50.4,20.7,-56.3,15.3C-62.2,9.8,-55.1,-2.9,-46.8,-11.1C-38.4,-19.3,-28.9,-23,-20.9,-27.8C-12.9,-32.6,-6.5,-38.5,3.7,-43.5C13.8,-48.6,27.6,-52.8,32.5,-47Z",
//     ],
//   },
//   {
//     top: "400px",
//     left: "-200px",
//     width: "500px",
//     height: "500px",
//     paths: [
//       "M47.6,-67.2C57.9,-58.1,59.8,-39.1,64.2,-21.8C68.5,-4.5,75.2,11,71.4,23.3C67.5,35.6,53.2,44.5,39.5,53.3C25.9,62.1,12.9,70.7,2.1,67.9C-8.8,65.1,-17.6,50.8,-29.1,41.2C-40.6,31.7,-54.7,27,-59.7,17.9C-64.8,8.7,-60.8,-4.9,-55.5,-17.1C-50.2,-29.3,-43.7,-40.2,-34.2,-49.6C-24.7,-59,-12.4,-67,3.2,-71.3C18.7,-75.7,37.3,-76.4,47.6,-67.2Z",
//       "M32.5,-47C37.4,-41.2,33.4,-25.4,37.7,-12.2C42.1,0.9,54.7,11.4,53.8,18.5C52.9,25.6,38.5,29.4,27.3,40.7C16,52,8,70.9,1.9,68.2C-4.2,65.6,-8.3,41.4,-19.9,30.2C-31.4,19,-50.4,20.7,-56.3,15.3C-62.2,9.8,-55.1,-2.9,-46.8,-11.1C-38.4,-19.3,-28.9,-23,-20.9,-27.8C-12.9,-32.6,-6.5,-38.5,3.7,-43.5C13.8,-48.6,27.6,-52.8,32.5,-47Z",
//       "M27.4,-35.7C41.3,-27.5,62.4,-27.3,62.2,-21.7C62,-16.1,40.4,-5,33,8.5C25.6,22,32.4,38,29.3,46.5C26.1,55.1,13.1,56.2,5.2,49C-2.6,41.8,-5.2,26.3,-12.5,19.1C-19.9,11.9,-31.9,13.1,-36.6,9.3C-41.4,5.6,-38.9,-3.1,-32.9,-7.3C-26.9,-11.6,-17.5,-11.3,-11.4,-22.1C-5.4,-32.8,-2.7,-54.6,2,-57.4C6.7,-60.2,13.4,-43.9,27.4,-35.7Z",
//       "M27.6,-33.9C40.2,-28.9,57.7,-26.7,64.5,-18C71.4,-9.4,67.6,5.7,60.4,17.2C53.3,28.7,42.9,36.8,32.3,40.6C21.7,44.5,10.8,44.2,-2.4,47.6C-15.7,50.9,-31.4,57.8,-41.5,53.8C-51.7,49.8,-56.3,34.8,-62.9,19.3C-69.5,3.8,-78,-12.1,-77.4,-28.6C-76.8,-45,-67,-61.9,-52.6,-66.2C-38.1,-70.5,-19.1,-62.2,-5.8,-54.3C7.6,-46.4,15.1,-38.8,27.6,-33.9Z",
//       "M41.1,-55.1C53.3,-47.7,63.4,-35.8,69.8,-21.4C76.1,-7.1,78.9,9.7,71.6,20.5C64.3,31.3,47.1,36.2,33.5,44.6C19.9,53,9.9,64.9,-2.7,68.6C-15.3,72.3,-30.6,67.7,-40.7,58.2C-50.8,48.7,-55.8,34.2,-61.1,19.5C-66.4,4.8,-72,-10.2,-67.2,-20.8C-62.3,-31.4,-47,-37.5,-34.1,-44.7C-21.1,-51.9,-10.6,-60.2,1.9,-62.8C14.4,-65.5,28.9,-62.6,41.1,-55.1Z",
//       "M47.6,-67.2C57.9,-58.1,59.8,-39.1,64.2,-21.8C68.5,-4.5,75.2,11,71.4,23.3C67.5,35.6,53.2,44.5,39.5,53.3C25.9,62.1,12.9,70.7,2.1,67.9C-8.8,65.1,-17.6,50.8,-29.1,41.2C-40.6,31.7,-54.7,27,-59.7,17.9C-64.8,8.7,-60.8,-4.9,-55.5,-17.1C-50.2,-29.3,-43.7,-40.2,-34.2,-49.6C-24.7,-59,-12.4,-67,3.2,-71.3C18.7,-75.7,37.3,-76.4,47.6,-67.2Z",
//     ],
//   },
// ]);
</script>

<style scoped lang="scss">
.app-background {
  position: fixed;
  z-index: -1;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;

  .blob {
    position: absolute;
    filter: blur(30px);
  }
}
</style>
