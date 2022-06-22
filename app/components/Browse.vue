<template>
  <Page class="page">
    <ActionBar class="action-bar">
      <NavigationButton visibility="hidden" />
      <GridLayout columns="50, *">
        <Label class="action-bar-title" text="Gameify" colSpan="2" />
        <Label class="fas" text.decode="&#xf0c9;" @tap="onDrawerButtonTap" />
      </GridLayout>
    </ActionBar>

    <GridLayout class="page__content">
      <Label
        class="page__content-placeholder"
        :text="message"
        visibility="hidden"
      />
      <TextField
        class="page__content-search"
        v-model="textField"
        hint="Wpisz nazwę gry"
        secure="false"
        keyboardType="text"
        returnKeyType="done"
        @returnPress="onReturnPress($event)"
        @focus="onFocus($event)"
        @textChange="onTextChange($event)"
        autocorrect="false"
        maxLength="20"
      >
      </TextField>
      <Label class="page__content-icon fas" text.decode="&#xf002;" />
      <Label class="page__content-placeholder" :text="message" />

      <Label
        class="page__content-label"
        :text="textFieldValue"
        v-if="showComponentWhenTyped"
      />
      <ListView
        class="page__content-list"
        v-if="showComponentWhenTyped"
        for="item in gameListFiltered"
      >
        <v-template>
          <Button
            class="text-sm"
            :text="`${item.name}`"
            @tap="onTapButtonItem($event, item.name)"
          />
        </v-template>
      </ListView>
    </GridLayout>
  </Page>
</template>

<script>
import * as utils from "~/shared/utils";
import { SelectedPageService } from "../shared/selected-page-service";
import axios from "axios/dist/axios";
import { textProperty } from "@nativescript/core/ui/text-base";

export default {
  data() {
    return {
      textField: "",
      textFieldValue: "",
      showComponentWhenTyped: false,
      actualSearchedValue: "",
      gameList: [],
      gameListFiltered: [],
    };
  },
  mounted() {
    SelectedPageService.getInstance().updateSelectedPage("Browse");
  },
  computed: {
    message() {
      return "Wyszukaj gre";
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
      this.actualSearchedValue = this.textField;
    },
    onFocus(event) {
      this.getDataToFilter();
    },
    onTextChange(event) {
      if (this.textField == "") {
        return (this.showComponentWhenTyped = false);
      }
      this.filterData();
      if (this.gameListFiltered.length != 0) {
        this.textFieldValue = this.gameListFiltered[0].name;
        console.log("----------------------", this.gameListFiltered[0].name);
        return (this.showComponentWhenTyped = true);
      }
      this.textFieldValue = "Brak podanej gry";
    },
    onTapButtonItem(event, name) {
      console.log("Nacisnałeś button o nazwie:", name);
    },
    getDataToFilter() {
      if (this.gameList.length == 0) {
        axios
          .get("https://api.steampowered.com/ISteamApps/GetAppList/v2/")
          .then((response) => {
            this.gameList = response.data.applist.apps;
            console.log(this.gameList);
            console.log(
              "----------------------------------------------------------------------"
            );
          })
          .catch(function (error) {
            // TODO: CATCH ERROR WHEN SIGHT NOT AVAILABLE
            console.log(error);
          });
      }
    },
    filterData() {
      if (this.gameList.length != 0) {
        this.gameListFiltered = this.gameList.filter((game) => {
          return game.name.toLowerCase().includes(this.textField.toLowerCase());
        });
        console.log("gameListFiltered:\n", this.gameListFiltered);
      }
    },
  },
};
</script>

<style scoped lang="scss">
@import "@nativescript/theme/scss/variables/blue";
</style>
