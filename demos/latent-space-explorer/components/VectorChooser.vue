<!-- Contributed by David Bau, in the public domain -->

<template>
<div class="vectorlist">
  <div v-for="(vector, index) in vectors" class="vector">
    <input v-model="vector.text">
    <button @click="selectVector(index)">&rarr;</button>
    <button @click="deleteVector(index)">x</button>
  </div>
  <div class="operation">
  <button @click="saveVector()">Save current sample</button>
  </div>
  <div class="operation">
  <!-- TODO: Change this button to do something interesting -->
  <button @click="applyVectorMath()">Apply vector math</button>
  </div>
</div>
</template>

<script>
import {Array1D, ENV} from 'deeplearn';
const math = ENV.math;

export default {
  props: {
    selectedSample: { },
    model: { },
    vectors: { type: Array, default: () => [ { text: "0,0,0,0,0,0,0,0,0,0,0," +
      "0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0" } ] }
  },
  methods: {
    saveVector() {
      console.log(this.selectedSample);
      this.selectedSample.data().then(x =>
         this.vectors.push({ text: Array.prototype.slice.call(x).join(',') })
      );
    },
    deleteVector(index) {
      this.vectors.splice(index, 1);
    },
    selectVector(index) {
      this.$emit("select", { selectedSample:
           Array1D.new(this.vectors[index].text.split(',').map(parseFloat))});
    },
    // TODO: Add useful vector space operations here -->
    applyVectorMath() {
      this.$emit("select", { selectedSample:
           math.add(this.selectedSample,
               Array1D.new([0.1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,
                  0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0])) } )
    }
  }
}
/*
export default {
  components: {
    Sample
  },
  data() {
    return {
      width: 20,
      visible: true,
      letters: "ABCDEFG".split("")
    }
  },
  props: {
    modelData: { type: String, default: "A" },
    selectedSample: { },
    model: { },
    samples: { type: Array, default: () => [
      zero, crispSerif, serifItalic, serifBlackItalic, sansLight, casual,
      dotMatrix] }
  },
  watch: {
    model: function(val) {
      this.select(this.samples[0], true);
    }
  },
  methods: {
    deleteSample: function(index) {
      this.samples.splice(index, 1);
    },
    select: function(sample, isInitialSelection) {
      this.$emit("select", {selectedSample: sample, isInitialSelection});
    },
    save: function() {
      this.samples.push(this.selectedSample);
    }
  }
}
*/
</script>

<style scoped>
.vector, .operation {
  border-top: 1px solid rgba(0, 0, 0, 0.1);
  white-space: nowrap;
}

</style>
