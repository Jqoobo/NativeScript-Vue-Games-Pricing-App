<template>
  <Page class="page">
    <ActionBar class="action-bar">
      <NavigationButton visibility="hidden" />
      <GridLayout columns="50, *">
        <Label class="action-bar-title" text="Wyszukiwarka" colSpan="2" />

        <Label class="fas" text.decode="&#xf0c9;" @tap="onDrawerButtonTap" />
      </GridLayout>
    </ActionBar>

    <GridLayout class="page__content">
      <TextField
        v-model="textField"
        hint="Wpisz nazwę gry"
        secure="false"
        keyboardType="text"
        returnKeyType="done"
        @returnPress="onReturnPress($event)"
        autocorrect="false"
        maxLength="20"
      >
      </TextField>
      <Label class="text-red-500 font-sans" :text="textFieldValue" v-if="showComponentWhenTyped" />
      <Label class="page__content-icon far" text.decode="&#xf1ea;" />
      <Label class="page__content-placeholder" :text="message" />
    </GridLayout>
  </Page>
</template>

<script>
import * as utils from "~/shared/utils";
import { SelectedPageService } from "../shared/selected-page-service";

export default {
  data() {
    return {
      textField: "",
      showComponentWhenTyped: false,
      actualSearchedValue: "",
    };
  },
  mounted() {
    SelectedPageService.getInstance().updateSelectedPage("Browse");
  },
  computed: {
    message() {
      return "Wyszukiwarka";
    },
    textFieldValue() {
      return this.actualSearchedValue;
    },
  },
  methods: {
    onDrawerButtonTap() {
      utils.showDrawer();
    },
    onReturnPress(event) {
      console.log(this.textField);
      this.showComponentWhenTyped = true;
      this.actualSearchedValue = this.textField;
    },
  },
};
</script>

<style scoped lang="scss">
@import "@nativescript/theme/scss/variables/blue";
</style>
