<template>
  <ion-card id="card">
    <ion-card-header id="header">
      <ion-card-title id="title">Total {{ valueType }}</ion-card-title>
      <ion-card-subtitle id="subtitle">{{ valueType }} cases in total</ion-card-subtitle>
    </ion-card-header>
    <ion-card-content id="values">
      {{ totalValues }}
    </ion-card-content>
  </ion-card>
</template>

<script>
import {
  IonCard,
  IonCardContent,
  IonCardSubtitle,
  IonCardTitle,
  IonCardHeader,
} from "@ionic/vue";
import { warning, wifi } from "ionicons/icons";
import { defineComponent } from "vue";

export default defineComponent({
  name: "Card",
  props: {
    totalValues: {
      type: Number,
      required: true,
    },
    valueType: String,
  },

  components: {
    IonCard,
    IonCardContent,
    IonCardSubtitle,
    IonCardTitle,
    IonCardHeader,
  },

  setup() {
    return { warning, wifi };
  },
});
</script>


<style scoped>
#card {
  border-radius: 24px;
}

#title,#subtitle,#values{
  text-align: center;
}

#title{
  font-size: 30px;
}

#values{
  font-size: 20px;
}
</style>