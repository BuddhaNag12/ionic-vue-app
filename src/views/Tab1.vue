<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Covid 19 live data</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Covid 19 live data</ion-title>
        </ion-toolbar>
      </ion-header>

      <ion-loading
        :is-open="isLoading"
        cssClass="my-custom-class"
        message="Please wait..."
        :duration="isLoading"
        @onDidDismiss="isLoading = false"
      >
      </ion-loading>
      <ion-title align="center" id="CountryTitle">{{
        selectedCountry || "Global Data"
      }}</ion-title>
      <ion-grid>
        <ion-row>
          <ion-col size="12" size-sm>
            <Card :totalValues="covidData.totalConfirmed" valueType="Confirmed"
          /></ion-col>
          <ion-col size="12" size-sm>
            <Card :totalValues="covidData.totalRecovered" valueType="Recovered"
          /></ion-col>
          <ion-col size="12" size-sm>
            <Card :totalValues="covidData.totalDeaths" valueType="Deaths"
          /></ion-col>
        </ion-row>
      </ion-grid>
      <ion-item>
        <ion-label>Select by country</ion-label>
        <ion-select
          placeholder="Select One"
          @ionChange="(event) => checkByCountry(event)"
        >
          <ion-select-option
            v-for="(name, index) in covidData.countries"
            :key="index"
            :value="name"
            @ionChange="selectedCountry = $event.target.value"
            >{{ name }}</ion-select-option
          >
        </ion-select>
      </ion-item>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
  IonCol,
  IonGrid,
  IonRow,
  IonLoading,
  IonLabel,
  IonSelect,
  IonSelectOption,
  IonItem,
} from "@ionic/vue";

import { defineComponent } from "vue";

import Card from "@/components/card.vue";
import axios from "axios";
const api = "https://covid19.mathdro.id/api/";
const dailyApi = "https://covid19.mathdro.id/api/daily";

interface CovidData {
  countries: [];
  active: number;
  deaths: {
    total: number;
  };

  confirmed: {
    total: number;
  };
}

interface Countries {
  name: string;
}
export default defineComponent({
  name: "Home",
  components: {
    IonHeader,
    IonToolbar,
    IonTitle,
    IonContent,
    IonPage,
    Card,
    IonCol,
    IonGrid,
    IonRow,
    IonLoading,
    IonLabel,
    IonSelect,
    IonSelectOption,
    IonItem,
  },

  data() {
    return {
      covidData: {
        totalDeaths: 0,
        totalConfirmed: 0,
        totalRecovered: 0,
        countries: [""],
      },
      selectedCountry: "",
      isLoading: false,
    };
  },

  created() {
    this.getCovidData();
    this.getCountries();
  },

  methods: {
    checkByCountry(evt: any) {
      this.selectedCountry = evt.detail.value;
      axios
        .get(`${api}countries/${evt.detail.value}`)
        .then(({ data }) => {
          const resultData = data;
          this.covidData.totalDeaths = resultData.deaths.value;
          this.covidData.totalConfirmed = resultData.confirmed.value;
          const recovered: number =
            this.covidData.totalConfirmed - this.covidData.totalDeaths;
          this.covidData.totalRecovered = recovered;
        })
        .catch((err) => console.log(err));
    },
    getCountries: function () {
      axios.get(`${api}countries`).then((res) => {
        const allCountries = res.data.countries;
        allCountries.forEach((country: Countries) => {
          this.covidData.countries.push(country.name);
        });
      });
    },

    getCovidData: function () {
      this.isLoading = true;
      axios
        .get(dailyApi)
        .then(({ data }) => {
          this.isLoading = false;
          const resultData = data;
          //console.log(resultData);

          resultData.forEach((results: CovidData) => {
            this.covidData.totalDeaths = results.deaths.total;
            this.covidData.totalConfirmed = results.confirmed.total;
            const recovered: number =
              this.covidData.totalConfirmed - this.covidData.totalDeaths;
            this.covidData.totalRecovered = recovered;
          });
        })
        .catch((e: any) => {
          console.log(e);
          this.isLoading = false;
        });
    },
  },
});
</script>

<style scoped>
#title {
  text-transform: uppercase;
  font-size: 30px;
}
#CountryTitle {
  text-transform: uppercase;
  font-size: 30px;
  font-family: Cambria, Cochin, Georgia, Times, "Times New Roman", serif;
}
</style>