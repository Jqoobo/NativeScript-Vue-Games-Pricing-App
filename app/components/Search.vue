<template>
  <Page class="page">
    <ActionBar class="action-bar">
      <NavigationButton visibility="hidden" />
      <GridLayout columns="50, *">
        <Image class="logo" src="https://media.discordapp.net/attachments/910979743137009714/990368859540316211/logoGameifyBlack.png" colSpan="2" />
      </GridLayout>
    </ActionBar>
    <ScrollView>
      <StackLayout>
    <GridLayout rows="225,150,150,150,150,150" class="page__content-grid">
      <Header v-if="offerList.steam"
      class="page__content-title"
        row="0"
        :image-src="offerList.steam.appImg"
        :title="offerList.steam.appTitle"
      />
      <SingleItem v-if="offerList.steam"
        row="1"
        platform-name="Steam"
        :page-url="offerList.steam.linkToStore"
        :cost-value="offerList.steam.pricePLN"
        :image-source="imageSource_Steam"
        :image-shop="imageShopp"
      />
      <SingleItem v-if="offerList.g2a"
        row="2"
        platform-name="G2A"
        :page-url="offerList.g2a.linkToStore"
        :cost-value="offerList.g2a.pricePLN"
        :image-source="imageSource_G2A"
        :image-shop="imageShopp"
      />
      <SingleItem v-if="offerList.kinguin"
        row="3"
        platform-name="Kinguin"
        :page-url="offerList.kinguin.linkToStore"
        :cost-value="offerList.kinguin.pricePLN"
        :image-source="imageSource_Kinguin"
        :image-shop="imageShopp"
      />
      <SingleItem v-if="offerList.eneba"
        row="4"
        platform-name="Eneba"
        :page-url="offerList.eneba.linkToStore"
        :cost-value="offerList.eneba.pricePLN"
        :image-source="imageSource_Eneba"
        :image-shop="imageShopp"
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
      imageSource_Steam: "https://cdn.discordapp.com/attachments/910979743137009714/990337822382780436/steam3.png",
      imageSource_G2A: "https://cdn.discordapp.com/attachments/910979743137009714/990338137521791036/g2a.png",
      imageSource_Kinguin: "https://media.discordapp.net/attachments/910979743137009714/990342046919323658/kinguin.png",
      imageSource_Eneba: "https://cdn.discordapp.com/attachments/910979743137009714/990337592815923250/eneba.png",
      imageShopp: "https://pngimg.com/uploads/shopping_cart/shopping_cart_PNG4.png",
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
