<template>
  <Page class="page">
    <ActionBar class="action-bar">
      <NavigationButton visibility="hidden" />
      <GridLayout columns="50, *">
        <Label class="action-bar-title" text="Gierka" colSpan="2" />
        <Label class="fas" text.decode="&#xf0c9;" @tap="onDrawerButtonTap" />
      </GridLayout>
    </ActionBar>
    <ScrollView>
      <StackLayout>
    <GridLayout rows="100,100,100,100,100" class="page__content-grid">
      <Header v-if="offerList.steam"
        row="0"
        :image-src="offerList.steam.appImg"
        :title="offerList.steam.appTitle"
      />
      <SingleItem v-if="offerList.steam"
        row="1"
        platform-name="Steam"
        :page-url="offerList.steam.linkToStore"
        :cost-value="offerList.steam.pricePLN"
      />
      <SingleItem v-if="offerList.g2a"
        row="2"
        platform-name="G2A"
        :page-url="offerList.g2a.linkToStore"
        :cost-value="offerList.g2a.pricePLN"
      />
      <SingleItem v-if="offerList.kinguin"
        row="3"
        platform-name="Kinguin"
        :page-url="offerList.kinguin.linkToStore"
        :cost-value="offerList.kinguin.pricePLN"
      />
      <SingleItem v-if="offerList.eneba"
        row="4"
        platform-name="Eneba"
        :page-url="offerList.eneba.linkToStore"
        :cost-value="offerList.eneba.pricePLN"
      />
    </GridLayout>
      </StackLayout>
    </ScrollView>
  </Page>
</template>

<script>
import * as utils from "~/shared/utils";
import { SelectedPageService } from "../shared/selected-page-service";
import axios from "axios/dist/axios";
import SingleItem from "./SingleItem.vue";
import Header from "./Header.vue";

export default {
  props: {
    url: String,
  },
  components: { SingleItem, Header },
  data() {
    return {
      offerList: [],
    };
  },
  mounted() {
    SelectedPageService.getInstance().updateSelectedPage("Search");
    this.getGamesInformationAndStores();
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
    async getGamesInformationAndStores() {
      console.log("Wysłano zapytanie\n", this.$props.url);
      axios
        .get(
          this.$props.url
          //"https://fdeb-193-192-177-31.eu.ngrok.io/api/product?item=minecraft"
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
.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}
@import "@nativescript/theme/scss/variables/blue";
</style>
