<template class="template">
  <StackLayout class="page__content-headers" orientation="vertical" width="1650px" height="600px" >
    <Label class="label_header" :text="title" width="500px" horizontal-align="center"/>
    <Image :src="imageSrc" width="1500px" />
  </StackLayout>
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
