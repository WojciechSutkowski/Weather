<script setup lang="ts">
import { ref } from 'vue';
import { WeatherTypes } from './types/WeatherTypes';

const apiKey = 'ad36321557110c7f975fe5a74cb85cb6';

const city = ref<string>('');
// const weather: Ref<WeatherTypes | null> = ref(null);
const weather = ref<WeatherTypes | null>(null);

const fetchWeather = () => {
  try {
    const url = `https://api.openweathermap.org/data/2.5/weather?q=${city.value}&appid=${apiKey}&units=metric`;
    fetch(url)
      .then((response) => response.json())
      .then((data: WeatherTypes) => {
        weather.value = data;
        console.log(weather.value);

        handleBackground(weather.value.weather[0].main);
      });
  } catch (e) {
    console.log('Error' + e);
  }
};

const handleBackground = (desc: String) => {
  const lowerDesc = desc.toLowerCase();
  const app = document.querySelector('#app') as HTMLElement;

  if (lowerDesc === 'clear')
    app.style.backgroundImage = `url("https://cdn.pixabay.com/photo/2018/08/06/22/55/sun-3588618__480.jpg")`;
  else if (lowerDesc === 'clouds')
    app.style.backgroundImage = `url('https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Cloudy_sky_%2826171935906%29.jpg/1200px-Cloudy_sky_%2826171935906%29.jpg')`;
  else if (lowerDesc === 'rain')
    app.style.backgroundImage = `url('https://www.rd.com/wp-content/uploads/2016/09/03-not-weird-facts-rain-Mr_Twister.jpg')`;
  else if (lowerDesc === 'snow')
    app.style.backgroundImage = `url('https://img.rasset.ie/0015d72f-1600.jpg')`;
};
</script>

<template>
  <main class="main">
    <div class="search-container">
      <input
        class="search-container__input"
        type="text"
        placeholder="Browse for weather..."
        v-model="city"
        @keypress.enter="fetchWeather"
      />
    </div>

    <div class="results" v-if="weather">
      <div class="location-container">
        <h1 class="heading">
          {{ weather.name }}
          <sup class="heading__country">{{ weather.sys.country }}</sup>
        </h1>
      </div>
      <div class="weather-container">
        <p class="weather-container__info weather-container__info--temp">
          {{ Math.round(weather.main.temp) }} ℃
        </p>
        <p class="weather-container__description">
          {{ weather.weather[0].description }}
        </p>
        <p class="weather-container__info">
          Feeling: {{ Math.round(weather.main.feels_like) }} ℃
        </p>
        <p class="weather-container__info">
          Humidity: {{ weather.main.humidity }} %
        </p>
        <p class="weather-container__info">
          Pressure: {{ weather.main.pressure }} hPa
        </p>
      </div>
    </div>
  </main>
</template>

<style lang="scss" scoped>
main {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 2rem;

  width: 100%;
  max-width: 120rem;
  margin: 0 auto;
  padding-top: 12.8rem;
}

.search-container {
  display: flex;
  justify-content: center;
  gap: 1.2rem;
  width: 100%;

  &__input {
    width: 100%;
    max-width: 48rem;
    font-size: 2.4rem;
    padding: 0.4rem 0.8rem;
    background-color: #ddd;
    border-radius: 5px;
    border: 2px solid #999;
    color: #000;

    &::placeholder {
      color: #000;
    }
  }
}

.results {
  display: flex;
  flex-direction: column;
  gap: 2rem;
  width: 100%;
  max-width: 56rem;
  background-color: #efefef;
  padding: 1.6rem 3.2rem;
  border-radius: 5px;
}
.location-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  background-color: #efefef;
  padding: 1.6rem 3.2rem;
  border-radius: 5px;
}

.heading {
  font-size: 4.8rem;
  font-weight: normal;

  &__country {
    font-size: 1.4rem;
    font-weight: 500;
    padding: 0.4rem 0.8rem;
    background-color: #fcad02;
    border-radius: 5px;
    border: 1px solid black;
  }
}
.weather-container {
  display: flex;
  flex-direction: column;
  gap: 1.2rem;

  &__description {
    font-size: 3.2rem;
    align-self: center;
  }

  &__info {
    font-size: 2rem;

    &--temp {
      align-self: center;
      font-size: 6.4rem;
    }
  }
}
</style>
