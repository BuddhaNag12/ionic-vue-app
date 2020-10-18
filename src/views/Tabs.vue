<template>
  <ion-page>
    <ion-tabs>
      <slot name="bottom">
        <ion-tab-bar>
          <ion-tab-button tab="Home" href="/tabs/tab1">
            <ion-icon :icon="homeOutline" />
            <ion-label>Home</ion-label>
          </ion-tab-button>

          <ion-tab-button tab="tab2" href="/tabs/tab2">
            <ion-icon :icon="heartHalfOutline" />
            <ion-label>Content</ion-label>
          </ion-tab-button>

          <ion-tab-button tab="tab3" href="/tabs/tab3">
            <ion-icon :icon="informationOutline" />
            <ion-label>About</ion-label>
          </ion-tab-button>
        </ion-tab-bar>
      </slot>
    </ion-tabs>
  </ion-page>
</template>

<script lang="ts">
import {
  IonTabBar,
  IonTabButton,
  IonTabs,
  IonLabel,
  IonIcon,
  IonPage,
} from "@ionic/vue";
import { homeOutline,informationOutline,heartHalfOutline } from "ionicons/icons";

export default {
  name: "Tabs",
  components: { IonLabel, IonTabs, IonTabBar, IonTabButton, IonIcon, IonPage },
  setup() {
    return {
     homeOutline,
     informationOutline,
     heartHalfOutline
    };
  },
};
</script>