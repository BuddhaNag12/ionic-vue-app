<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Covid 19 traker</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">{{
            selectedCountry? selectedCountry : "Global Data"
          }}</ion-title>
        </ion-toolbar>
      </ion-header>

      <ion-title align="center" id="CountryTitle">{{
        selectedCountry ? selectedCountry : "Global Data"
      }}</ion-title>

      <ion-loading
        :is-open="isLoading"
        cssClass="my-custom-class"
        message="Please wait..."
      >
      </ion-loading>
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
        <ion-item>
          <ion-label>Select by country</ion-label>
          <ion-select
            placeholder="Select One"
            @ionChange="(event) => checkByCountry(event)"
          >
            <ion-select-option
              v-for="name in covidData.countries"
              :key="name"
              :value="name"
              >{{ name }}</ion-select-option
            >
          </ion-select>
        </ion-item>
      </ion-grid>
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
  IonLabel,
  IonSelect,
  IonSelectOption,
  IonItem,
  IonLoading,
} from "@ionic/vue";

import { defineComponent } from "vue";

import Card from "@/components/MyCard.vue";
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
    IonLabel,
    IonSelect,
    IonSelectOption,
    IonItem,
    IonLoading,
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
    checkByCountry: function (evt: any) {
      this.isLoading = true;
      this.selectedCountry = evt.detail.value;
      const CountryName = evt.detail.value;
      // this.selectedCountry = CountryName;
      if (CountryName) {
        axios
          .get(`${api}countries/${CountryName}`)
          .then(({ data }) => {
            const ResultData = data;
            this.isLoading = false;
            this.covidData.totalDeaths = ResultData.deaths.value;
            this.covidData.totalConfirmed = ResultData.confirmed.value;
            const recovered: number =
              this.covidData.totalConfirmed - this.covidData.totalDeaths;
            this.covidData.totalRecovered = recovered;
          })

          .catch((err) => {
            this.isLoading = false;
            console.log("error", err);
          });
      }
    },
    getCountries: function () {
      axios.get(`${api}countries`).then((res) => {
        const EachCountryName = res.data.countries;
        EachCountryName.forEach((country: Countries) => {
          this.covidData.countries.push(country.name);
        });
      });
    },

    getCovidData: function () {
      this.isLoading = true;
      axios
        .get(dailyApi)
        .then(({ data }) => {
          const resultData = data;
          this.isLoading = false;
          resultData.forEach((results: CovidData) => {
            this.covidData.totalDeaths = results.deaths.total;
            this.covidData.totalConfirmed = results.confirmed.total;
            const recovered: number =
              this.covidData.totalConfirmed - this.covidData.totalDeaths;
            this.covidData.totalRecovered = recovered;
          });
        })
        .catch((e: any) => {
          this.isLoading = false;
          console.log(e);
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
  margin-top: 0.6rem;
  text-transform: uppercase;
  font-size: 30px;
  font-family: sans-serif;
}

#spinner {
  align-self: center;
}
</style>