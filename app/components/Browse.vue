<template>
  <Page class="page">
    <ActionBar class="action-bar">
      <NavigationButton visibility="hidden" />
      <GridLayout columns="50, *">
        <Image class="logo" src="https://media.discordapp.net/attachments/910979743137009714/990368859540316211/logoGameifyBlack.png" colSpan="2" />
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
        hint="Wpisz nazwę gry..."
        secure="false"
        keyboardType="text"
        returnKeyType="done"
        @returnPress="onReturnPress($event, textField)"
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
            class="page__content-button"
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
import Search from "./Search.vue";

export default {
  components: { Search },
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
      return "Wyszukaj grę...";
    },
    textFieldValue() {
      return this.actualSearchedValue;
    },
  },
  methods: {
    onDrawerButtonTap() {
      utils.showDrawer();
    },
    onReturnPress(event, name) {
      if(this.textField!="")
      {
      this.actualSearchedValue = this.textField;
      const itemUrl = encodeURI("https://beab-193-19-165-79.eu.ngrok.io/api/product?item=" +name);
      this.$navigateTo(Search, {
        transition: {
          name: "slidetOP",
          duration: 300,
          curve: "easeIn" },
        props: { url: itemUrl }
      })
      return;
      }
      alert({
        title: "Hej!",
        message: "Nie wpisano żadnej gry!",
        okButtonText: "Zamknij"
      }) 
      .then(() => {
      console.log("Alert dialog closed.");
  });
      
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
      const itemUrl = encodeURI("https://beab-193-19-165-79.eu.ngrok.io/api/product?item=" +name);
      console.log("Nacisnałeś button o nazwie:", name, "i urlu: ", itemUrl);
      this.$navigateTo(Search, {
      transition: {
        name: "slidetOP",
        duration: 300,
        curve: "easeIn" },
      props: { url: itemUrl }
      });
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
}
}
</script>

<style scoped lang="scss">
@import "@nativescript/theme/scss/variables/blue";
</style>
