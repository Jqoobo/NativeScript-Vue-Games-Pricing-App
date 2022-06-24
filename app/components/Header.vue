<template>
  <GridLayout columns="*,300,*" rows="50, *, 50">
    <Image :src="imageSrc" row="0" col="1" backgroundColor="green" />
    <Label :text="title" row="2" col="1" backgroundColor="green" />
  </GridLayout>
</template>

<script>
export default {
  props: {
    imageSrc: String,
    title: String,
  },

  data() {
    return {};
  },
  mounted() {},
  computed: {},
  methods: {},
};
</script>

<style scoped lang="scss">
@import "@nativescript/theme/scss/variables/blue";
</style>
