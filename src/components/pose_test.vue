<script setup>
import { ref, onMounted } from 'vue';
import { proto } from './proto_demo.mjs';

const faces = [
  'face_up',
  'face_down',
  'right',
  'left',
  'vertical_up',
  'vertical_down',
];
//faces array contains the 6 possible poses
let instrument_settings = (i) => {
  let n = 1;
  //n is set to 1
  let calc = (x) => (n > 0 ? x - 1 : x);
  //calc is a function that takes in x and returns x - 1 if n is greater than 0, otherwise it returns x
  let [f, s, v] = [
    calc(i.poses.faces),
    calc(i.poses.sides),
    calc(i.poses.vertical),
  ];
  // f, s, and v are set to the results of calc with the corresponding pose values from the proto array of objects

  let face_option =
    f > 2 ? [faces[1]] : f > 1 ? [faces[0]] : f > 0 ? [faces[0], faces[1]] : [];
  // if f is more than 2, face_option will be the second element in the faces array, if it (f) is greather than 1, face_option will be the first element (faces[0]), if f is greater than 0, both elements of the array will be shown, otherwise face_option will be an empty array.

  let sides =
    s > 2 ? [faces[2]] : s > 1 ? [faces[3]] : s > 0 ? [faces[3], faces[2]] : [];

  //if s is more than 2, the third element in the faces array will be shown, else if s is greater than 1, the fourth element will be shown, if s is more than 0, both the third and fourth elements will be shown, otherwise sides will be a empty array.

  let vertical =
    v > 2 ? [faces[5]] : v > 1 ? [faces[4]] : v > 0 ? [faces[4], faces[5]] : [];

  //doing the same thing as the previous 2, where if v is greater than 2, the sixth element of the faces array will be shown, if v is greater than 1, the fifth element will be shown and if it is greater than 0 then both the fifth and sixth element will be shown, otherwise vertical will be a empty array.

  return [...face_option, ...sides, ...vertical];

  // this will show the arrays above in order of face_option, sides and vertical, so basically the options are face_option is empty, sides and vertical will show, sides is empty, face_option and vertical will show and vertical is empty, face_option and sides will show and if all of them are somehow empty then nothing will be shown, otherwise, all 3 will be shown.
};

const cubes = ref([]);
const hl = ref([]);
// both  is referencing to an empty array

//store selected face of each item in a state
const faceSelectedByUser = ref([]);

//store the selected poses in a state
const selectedPoses = ref([]);

/*
const chng_face = (i, face) => {
  let c = cubes.value.filter((ii) => ii[0] != i);
  let h = hl.value.filter((ii) => ii[0] != i);
  c.push([i, `show-${face}`]);
  h.push([i, face]);
  cubes.value = c;
  hl.value = h;
};
*/

const chng_face = (i, event) => {
  faceSelectedByUser.value[i] = event.target.value;
};

// the face is the face of the cube and i is the index, these are taken as arguements for the function,c is set to the empty cubes array (initial state) from before and h to the hl array, both are then filtered to show the index that is not equal to arguemnt i (ii => ii[0] != i), then pushed into the c and h arrays, which cubes and hl is set to (similar to setCube, setHl).

//changed this to get the value of the selected side from the dropdown
const getCube = (i) => {
  const selected = faceSelectedByUser.value[i];
  return selected ? `show-${selected}` : null;
};

// this functions gets the cube, where c is filtered for the arguemented index, (if the arguement index is the same as the index in the array, then show it), then if this length of the cube is greater than 0 (c.length > 0), show the first element other wise show nothing (null)

const getHL = (i, face) => {
  let h = hl.value.filter((ii) => ii[0] == i);
  return (
    h.length > 0 &&
    h[0][1] == face &&
    faceSelectedByUser.value[i]?.includes(face)
  );
};

// this function gets the highlights, where i is the index and face is the face of the cube, h is set to the aforementioned hl array and does the same thing as before where it filters hl for the arguement index, and if the length of h is greater AND the first element of h is the face, then it will return true, otherwis it will return false.


//REDO THIS

const filterEntry = (entry) => {
  if (selectedPoses.value.length === 0) {
    return true; // Display all entries if no pose options are selected
  } else {
    const poses = instrument_settings(entry.settings);
    return selectedPoses.value.every((pose) => poses.includes(pose));
  }

};

</script>

<template>
  <td>
    <div>
      <label v-for="pose in faces" :key="pose">
        <input type="checkbox" :value="pose" v-model="selectedPoses" />
        {{ pose }}
      </label>
    </div>
  </td>
  <br />
  <br />
  <table class="list">
    <th>face</th>
    <th>options</th>
    <th>result</th>

    <template v-for="(item, idx) of proto">
    <tr v-if="filterEntry(item)">
      <tr class="title space">
        <td colspan="3">{{ '|   ' }}</td>
      </tr>

      <tr class="title">
        <td colspan="3">{{ item.scalpel_id }}</td>
      </tr>

      <tr class="v_up">
        <td>{{ idx }}</td>
        <td class="code">
          poses: {{ item.settings.poses }}
          <br />
          <br />
          time: {{ item.time }}
          <br />
          <br />
          properties: {{ item.properties }}
          <br />
          <br />
          Admin: {{ item.admin }}
        </td>
        <td>
          <div v-for="res of instrument_settings(item.settings)">{{ res }}</div>
        </td>
      </tr>

      <tr>
        <td>
          <div class="scene">
            <div :class="`cube ${getCube(idx)}`">
              <div
                v-for="pose in faces"
                :class="`cube__face cube__face--${pose}${
                  getHL(idx, pose) ? ' highlight' : ''
                }`"
              >
                {{ pose.split('_').join(' ') }}
              </div>
            </div>
          </div>
        </td>

        <td>
          <div style="display: flex; flex-direction: row">
            <div
              v-for="face in instrument_settings(item.settings)"
              class="scene2"
            >
              <div :class="`cube2 show-${face}`">
                <div
                  v-for="pose in faces"
                  :class="`cube2__face cube2__face--${pose}`"
                >
                  {{ pose == face ? String(pose).split('_').join(' ') : null }}
                </div>
              </div>
            </div>
          </div>

          <div style="display: flex; flex-direction: row">
            <select
              v-model="faceSelectedByUser[idx]"
              @change="chng_face(idx, $event)"
            >
              <option
                v-for="face in instrument_settings(item.settings)"
                :value="face"
              >
                {{ face }}
              </option>
            </select>
          </div>
        </td>
      </tr>
      </tr>
    </template>
  </table>
</template>

<style scoped lang="sass">

body
  font-size: 12px
  background-color: #242424

button
  margin: 2px

.list
  border-spacing: 0px
  .title
    border-spacing: 0
    font-size: large
  .v_up
    vertical-align: top
    .code
      color: #bebebe
      font-size: small

.space
  background-color: #242424
  color: #242424

.sect
  display: flex
  flex-direction: column
  max-width: 30vw
  min-width: 29vw
  margin: 20px

.sub
  color: #edff79

table
  padding: 0 10px
  text-transform: lowercase
  font-family: monospace

tr
  background-color: #183666

td
  padding: 10px 0 0 10px

.list td:hover
  background: red


.col
  display: flex
  flex-direction: column
  margin-right: 20px

.active
  background-color: #565ed8
</style>
