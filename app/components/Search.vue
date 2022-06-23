<template>
  <Page class="page">
    <ActionBar class="action-bar">
      <NavigationButton visibility="hidden" />
      <GridLayout columns="50, *">
        <Label class="action-bar-title" text="Gierka" colSpan="2" />
        <Label class="fas" text.decode="&#xf0c9;" @tap="onDrawerButtonTap" />
      </GridLayout>
    </ActionBar>
    <GridLayout rows="100,100,100,100" class="page__content-grid">
      <Button
        row="0"
        text="wypełnij"
        textAlignment="center"
        class="page__content-box4"
        @tap="uzupelnijButton($event)"
        style="background-color: cadetblue; margin: 5"
      />
      <Header
        v-if="offerList.steam"
        row="1"
        :image-src="offerList.steam.appImg"
        :title="offerList.steam.appTitle"
      />
      <SingleItem
        v-if="offerList.steam"
        row="2"
        platform-name="Steam"
        :page-url="offerList.steam.linkToStore"
        :cost-value="offerList.steam.pricePLN"
      />
      <!--<SingleItem v-if="offerList.g2a"
      row="3"
        platform-name="G2A"
        :page-url="offerList.g2a.linkToStore"
        :cost-value="offerList.g2a.pricePLN"
      />-->
      <!--<SingleItem platform-name="Kinguin"/>
      <SingleItem platform-name="Eneba"/>-->
    </GridLayout>
  </Page>
</template>

<script>
import * as utils from "~/shared/utils";
import { SelectedPageService } from "../shared/selected-page-service";
import axios from "axios/dist/axios";
import SingleItem from "./SingleItem.vue";
import Header from "./Header.vue";

export default {
  components: { SingleItem, Header },
  data() {
    return {
      gameNameUrl: "minecraft",
      offerList: [],
    };
  },
  mounted() {
    SelectedPageService.getInstance().updateSelectedPage("Search");
  },
  computed: {
    message() {
      return "";
    },
  },
  methods: {
    onDrawerButtonTap() {
      utils.showDrawer();
    },
    uzupelnijButton() {
      this.undefinedFunction();
    },
    async undefinedFunction() {
      console.log("Wysłano zapytanie");
      axios
        .get(
          "https://8d06-193-192-177-31.eu.ngrok.io/api/product?item=minecraft"
        )
        .then((response) => {
          console.log("Pobrano dane\n", response);
          this.offerList = response.data.site;
        })
        .catch(function (error) {
          console.log("Nie pobrano danych\n", error, "\n", error.request);
        });
    },
  },
};
</script>

<style scoped lang="scss">
@import "@nativescript/theme/scss/variables/blue";
</style>
