<template>
    <Page class="page">
      <ActionBar class="action-bar">
        <NavigationButton visibility="hidden"/>
        <GridLayout columns="50, *">
          <Label class="action-bar-title" text="Gierka" colSpan="2"/>

          <Label class="fas" text.decode="&#xf0c9;" @tap="onDrawerButtonTap"/>
        </GridLayout>
      </ActionBar>

        <GridLayout class="page__content">
            <Label class="page__content-placeholder" :text="message"/>
            <DockLayout>
                        <Image src="https://cdn.akamai.steamstatic.com/steam/apps/271590/header.jpg?t=1618856444" dock="top" text="Wielka Kradzież Aut 5" textAlignment="center" class="page__content-box" style="margin: 5;"></Image>                  
                        <label dock="bottom" text="Ceny:" textAlignment="center" class="page__content-box1" style="background-color: cadetblue; margin: 5;"></label>
                        <label text="No fajna gra 8/10" textAlignment="center" class="page__content-box2"  style="background-color: cadetblue; margin: 5;"></label>       
            </DockLayout>
        </GridLayout>
    </Page>
</template>

<script>
  import * as utils from "~/shared/utils";
  import { SelectedPageService } from "../shared/selected-page-service";

  export default {
    mounted() {
      SelectedPageService.getInstance().updateSelectedPage("Search");
    },
    computed: {
      message() {
        return "Pokaż gierke = true";
      }
    },
    methods: {
      onDrawerButtonTap() {
        utils.showDrawer();
      }
    }
  };
</script>

<style scoped lang="scss">
    // Start custom common variables
    @import '@nativescript/theme/scss/variables/blue';
    // End custom common variables

    // Custom styles
</style>
