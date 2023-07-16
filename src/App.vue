<script setup>
import Poses from './components/pose_test.vue';
import { proto } from './components/proto_demo.mjs';

//console.log(proto);
//shows proto array of objects
//console.log(proto[0].properties);
//shows properties of first object in proto array, so to get all of them, use a for loop to iterate through the array and get the properties of each object
//have to use v-for directive to iterate through the array and get the properties of each object
//source to revisit: https://www.tutorialspoint.com/how-to-write-and-use-for-loop-in-vue-js#:~:text=Looping%20through%20an%20Array,-Example&text=We%20use%20the%20v%2Dfor,value%20of%20each%20named%20item.&text=This%20is%20the%20main%20file,available%20inside%20the%20src%20folder.
</script>

<template>
  <div style="margin: 10px">
    Tech test - Tasks:
    <div>
      <div class="task">
        1. display all values given in the 'properties' object for each entry,
        use your judgment on how to display them
      </div>
      <div>
        <h3>Properties</h3>
        <ul v-for="(object, idx) in proto" :key="object.properties.sub">
          <h4>Tool {{ idx + 1 }}</h4>
          <p>
            The {{ object.properties.finer }}
            {{ object.properties.broader }} has a length of
            {{ object.properties.length }} inches,
            <template v-if="object.properties.characteristic">
              with a {{ object.properties.characteristic }} characteristic,
            </template>

            <template v-if="object.properties.speciality">
              and the specialities where it's use include:
              <br />

              <li v-for="thing in object.properties.speciality" :key="thing">
                {{ thing }}
              </li>
            </template>
            <br />
            <template v-if="object.properties.sub">
              This tools sub-category is {{ object.properties.sub }}
            </template>
          </p>
        </ul>
      </div>
      <div class="task">
        2. comment on each function in 'pose_test.vue' and explain what it does
        <br />(extra credit for commenting on the other functions in the file)
        <h3>Done, check comments</h3>
      </div>
      <div class="task">
        3. contain the contents of each entry within the blue container

        <h3>
          Done, put all the info inside the entries of the proto array in a blue
          container
        </h3>
      </div>
      <div class="task">
        4. convert the pose buttons to a dropdown menu using the vue composition
        api bindings to set the face of the cube (space-out contents of each
        entry as you see fit)
      </div>
      <div class="task">
        5. create filter checkboxes for each of the poses (face up, face down,
        right, left, vertical up, vertical down) and filter the list of entries
        based on the selected options
      </div>
      <div class="task">
        6. change the background color of each blue container on hover by using
        the vue action and binding the hover sass class
        <br />(you can change all the colours as you please)
      </div>
    </div>
    <div>
      Not all tasks need to be completed, but the more you do the better.
    </div>
  </div>

  <Poses />
</template>

<style scoped lang="sass">
.task
  margin: 10px
  font-size: 12px
  font-weight: bold
</style>
