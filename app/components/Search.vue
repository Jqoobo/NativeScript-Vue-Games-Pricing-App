<template>
  <Page class="page">
    <ActionBar class="action-bar">
      <NavigationButton visibility="hidden" />
      <GridLayout columns="50, *">
        <Label class="action-bar-title" text="Gierka" colSpan="2" />

        <Label class="fas" text.decode="&#xf0c9;" @tap="onDrawerButtonTap" />
      </GridLayout>
    </ActionBar>

    <GridLayout class="page__content">
      <Label class="page__content-placeholder" :text="message" />
      <DockLayout>
        <Image
          :src="imageSrc"
          dock="top"
          text="GTA 5"
          textAlignment="center"
          class="page__content-box"
          style="margin: 5"
        ></Image>
        <label
          dock="top"
          :text="nameTextField"
          textAlignment="center"
          class="page__content-box1"
          style="background-color: cadetblue; margin: 5"
        ></label>
        <label
          dock="left"
          :text="costValueTextField"
          textAlignment="center"
          class="page__content-box2"
          style="background-color: cadetblue; margin: 5"
        ></label>
        <label
          dock="right"
          :text="platformTextField"
          textAlignment="center"
          class="page__content-box3"
          style="background-color: cadetblue; margin: 5"
        ></label>
        <Button
          dock="bottom"
          :text="buyButtonText"
          textAlignment="center"
          class="page__content-box4"
          @tap="onTapButtonBuy($event)"
          style="background-color: red; margin: 5"
        />
      </DockLayout>
    </GridLayout>
  </Page>
</template>

<script>
import * as utils from "~/shared/utils";
import { SelectedPageService } from "../shared/selected-page-service";
import axios from "axios/dist/axios";

export default {
  data() {
    return {
      imageSrc:
        "https://cdn.akamai.steamstatic.com/steam/apps/271590/header.jpg?t=1618856444",
      buyButtonText: "Dokonaj zakupu/przejdz na stronę",
      nameTextField: "Grannnn Deeeff Outooo Faaaaaiv",
      platformTextField: "Steam",
      costValueTextField: "21,37 zł",
      offerList: [],
    };
  },
  mounted() {
    SelectedPageService.getInstance().updateSelectedPage("Search");
  },
  computed: {
    message() {
      return "Pokaż gierke = true";
    },
  },
  methods: {
    onDrawerButtonTap() {
      utils.showDrawer();
    },
    async undefinedFunction() {
      console.log("Wysłano zapytanie");
      axios
        .get(
          "https://16b6-193-192-177-31.eu.ngrok.io/api/product?item=minecraft"
        )
        .then((response) => {
          console.log("Pobrano dane\n", response);
          this.offerList = response.data;
          if (this.offerList.site.steam) {
            this.platformTextField = "Steam";
            this.imageSrc = this.offerList.site.steam.appImg;
            this.nameTextField = this.offerList.site.steam.appTitle;
            this.costValueTextField =
              "Ceny od " + this.offerList.site.steam.pricePLN / 100 + " zł";
          }
        })
        .catch(function (error) {
          console.log("Nie pobrano danych\n", error, "\n", error.request);
        });
    },
    onTapButtonBuy(event) {
      console.log("-----------------------------------------");
      this.undefinedFunction();
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
