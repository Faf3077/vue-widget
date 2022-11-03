<template>
  <div id="app">
    <main>
      <!-- Поиск -->
      <div class="search-box">
        <input type="text" v-model="capital" @keypress="dataWeather" />
      </div>

      <!-- Результат -->
      <div
        class="weather__wrapper"
        :class="{ Hot: hotWeather, cold: coldWeather }"
        v-if="typeof weather.name != 'undefined'"
        ref="weather__wrapper"
      >
        <div class="block__first">
          <div class="city">{{ weather.name }}</div>
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
        </div>
        <div class="block__second">
          <div class="wind">{{ weather.wind.speed }} м/с</div>
          <div class="description">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      weather: {},
      hotWeather: false,
      coldWeather: false,
      capital: "",
    };
  },

  methods: {
    dataWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `http://api.openweathermap.org/data/2.5/weather?q=${this.capital}&units=metric&APPID=5e425b2d5954bd1f8a2d5eb116e4f4a5`
        )
          .then((data) => {
            return data.json();
          })
          .then(this.dataItem);
      }
    },

    // Результат
    dataItem(item) {
      this.weather = item;
      //изменение цвета взависимости от температуры
      if (Math.round(item.main.temp) > 20) {
        this.hotWeather = true;
        this.coldWeather = false;
      } else if (Math.round(item.main.temp) < 0) {
        this.coldWeather = true;
        this.hotWeather = false;
      } else {
        this.coldWeather = false;
        this.hotWeather = false;
      }
    },
  },
};
</script>

<style>
.weather__wrapper {
  width: 300px;
  height: 300px;
  text-align: center;
  margin: 15% auto;
  border: 1px solid black;
  border-radius: 10px;
}
.search-box {
  display: flex;
  justify-content: center;
  margin: 0 auto;
}
input {
  width: 300px;
  background: -webkit-linear-gradient(top, #c8ffc4, #afb8ff);
  border: 2px solid #999;
  border-radius: 3px;
  box-shadow: 0 0 5px #999;
  color: #111;
  font-size: 15px;
  padding: 10px;
}
.block__first {
  font-weight: 800;
  font-size: 28px;
  margin: 60px 0 30px;
}

.block__second {
  font-size: 18px;
}
.Hot {
  background: #ffffcc;
}
.cold {
  background: #ccffff;
}
</style>
