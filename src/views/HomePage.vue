<template>
  <ion-page>
    <ion-content :fullscreen="true">
      <div class="main-container">
        <div class="refresh-container">
          <ion-button mode="ios" color="primary" @click="fetchData">
            Refresh
          </ion-button>
        </div>

        <div class="crypto-list">
          <div v-for="coin in coins" :key="coin.id" class="crypto-card">
            <ion-grid class="no-padding">
              <ion-row class="ion-align-items-center">
                <ion-col size="2" class="col-rank">
                  <div class="label-text">Rank</div>
                  <div class="value-text">{{ coin.rank }}</div>
                </ion-col>

                <ion-col size="5" class="col-name">
                  <div class="label-text">{{ coin.name }}</div>
                  <div class="value-text">{{ coin.symbol }}</div>
                </ion-col>

                <ion-col size="5" class="col-price">
                  <div class="label-text">USD</div>
                  <div class="value-text">{{ coin.price_usd }}</div>
                </ion-col>
              </ion-row>
            </ion-grid>
          </div>
        </div>
      </div>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import {
  IonPage,
  IonContent,
  IonButton,
  IonGrid,
  IonRow,
  IonCol,
} from "@ionic/vue";
import { ref, onMounted } from "vue";

// State data
const coins = ref<any[]>([]);

// Fungsi Ambil Data API
const fetchData = async () => {
  try {
    const response = await fetch("https://api.coinlore.net/api/tickers/");
    const data = await response.json();

    if (data && data.data) {
      coins.value = data.data;
    }
  } catch (error) {
    console.error("Error fetching data:", error);
  }
};

// Jalankan saat halaman dibuka
onMounted(() => {
  fetchData();
});
</script>

<style scoped>
/* Layout dasar */
.main-container {
  background-color: #fff;
  min-height: 100%;
  padding-top: 20px;
}

.refresh-container {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.crypto-list {
  padding: 0 16px;
}

/* Styling Card ala Gambar Referensi */
.crypto-card {
  background-color: #fff8dc; /* Warna kekuningan/krem */
  border-bottom: 2px solid #e0d8b0;
  padding: 12px 8px;
  margin-bottom: 6px;
  border-radius: 6px;
}

/* Font Styling */
.label-text {
  font-size: 11px;
  color: #555;
  margin-bottom: 4px;
  font-weight: 500;
}

.value-text {
  font-size: 18px;
  color: #000;
  font-weight: 700;
}

/* Grid Helper */
.no-padding {
  padding: 0;
  --ion-grid-padding: 0;
}

.col-rank {
  text-align: center;
}

.col-name {
  padding-left: 12px;
}

.col-price {
  text-align: left; /* Sesuaikan kiri seperti gambar */
}
</style>
