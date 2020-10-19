<template>
  <ion-card id="card" :class="getBorderType(valueType)">
    <ion-card-header id="header">
      <ion-card-title id="title">Total {{ valueType }}</ion-card-title>
      <ion-card-subtitle id="subtitle"
        >{{ valueType }} cases in total</ion-card-subtitle
      >
    </ion-card-header>
    <ion-card-content id="values">
      {{ totalValues }}
    </ion-card-content>
  </ion-card>
</template>

<script lang="ts">
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
    function getBorderType(valueType: string) {
      if (valueType == "Deaths") {
        return "deaths-top";
      } else if (valueType == "Recovered") {
        return "recovered-top";
      } else {
        return "active-top";
      }
    }
    return { warning, wifi, getBorderType };
  },
});
</script>


<style scoped>
#card {
  border-radius: 24px;
}

#title,
#subtitle,
#values {
  text-align: center;
}

#title {
  font-size: 30px;
}

#values {
  font-size: 20px;
}
.deaths-top {
  border-top: 2px solid red;
}
.recovered-top {
  border-top: 2px solid green;
}
.active-top {
  border-top: 2px solid lightcoral;
}
</style>