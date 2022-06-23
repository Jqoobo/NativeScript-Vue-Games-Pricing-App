<template>
  <GridLayout columns="*,150,*,150,*" rows="50, *, 50">
    <Label :text="`${costValue/100} zł`" row="0" col="1" backgroundColor="green" />
    <Label :text="platformName" row="0" col="3" backgroundColor="green" />
    <Button class="text-sm"
      :text="buyButtonText"
      row="2"
      col="1"
      colSpan="3"
      backgroundColor="green"
      @tap="buyButton($event)"
    />
  </GridLayout>
</template>

<script>
import { Utils } from "@nativescript/core";
export default {
  props: {
    platformName: String,
    pageUrl: String,
    costValue: Number,
  },

  data() {
    return {
      buyButtonText: "Przejdź na stronę",
    };
  },
  mounted() {},
  computed: {},
  methods: {
    buyButton(event) {
      Utils.openUrl(this.$props.pageUrl);
    },
  },
};
</script>

<style scoped lang="scss">
// Start custom common variables
@import "@nativescript/theme/scss/variables/blue";
// End custom common variables

// Custom styles
</style>
