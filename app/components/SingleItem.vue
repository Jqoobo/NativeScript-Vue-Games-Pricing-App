<template>
  <GridLayout class="items" columns="*,150,*,150,*" rows="50, *, 50">
    <Label
    class="group relative w-full flex justify-center py-3 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-indigo-800 hover:bg-indigo-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
      :text="`${costValue / 100}zł`"
      row="0"
      col="1"
    />
    <Label 
    class="group relative w-full flex justify-center py-3 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-indigo-800 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
    row="0" 
    col="3"  
    >
    </Label>
    <Label
      class="group relative w-full flex justify-center py-3 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-teal-500 dark:bg-teal-500 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
      :text="buyButtonText"
      row="2"
      col="1"
      colSpan="3"
      @tap="buyButton($event)"
    />
    <Image row="0" col="3" class="pic1 " :src="imageSource"/>
    <Image row="2" col="1" colSpan="3" class="pic2" :src="imageShop"/>
  </GridLayout>
</template>

<script>
import { Utils } from "@nativescript/core";
export default {
  props: {
    platformName: String,
    pageUrl: String,
    costValue: Number,
    imageSource: String,
    imageShop: String,
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
@import "@nativescript/theme/scss/variables/blue";
</style>
