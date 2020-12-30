<template>
  <div>
    <cat v-for="c in cats" :key="c.slug" :slug="c.slug" :indent="c.indent" :cat="c">
      <!-- <template
        v-if="cats.val2 && cats.val2[c.slug] && cats.val2[c.slug].length"
      >
        <cats :cats="{ val1: cats.val2[c.slug] }"
      /></template> -->
    </cat>
  </div>
</template>

<script>
import Cat from "../components/Cat";

const Cats = {
  name: "cats",
  components: {
    Cat
  },
  props: {
    cats: {
      type: Array,
      required: true
    }
  }
};
export default Cats;
</script>

<style></style>
