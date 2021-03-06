<template>
  <div class="modal">
    <transition name="fade" appear>
      <div v-if="showModal" class="modal__overlay" @click="closeModal()"></div>
    </transition>

    <transition name="slide" appear>
      <div class="modal__content" v-if="showModal">
        <h2 class="modal__title">{{message.title}}</h2>
        <h4 class="modal__body">{{message.body}}</h4>
        <hr />
        <p class="modal__location">The {{weather.location}} Weather Report</p>
        <p class="modal__status">Sky⛅️ : {{weather.status}}</p>
        <p class="modal__temp">Tempreture🌡 : {{weather.temp}}C</p>
        <p class="modal__pressure">Pressure📊 : {{weather.pressure}}mmHg</p>
        <p class="modal__humidity">Humidity💧 : {{weather.humidity}}</p>
        <button class="form__btn" @click="closeModal()">Close</button>
      </div>
    </transition>
  </div>
</template>

<script>
import { getWeather } from "@/WeatherService.js";

export default {
  name: "modal",
  props: ["formState"],
  data() {
    return {
      showModal: true,
      message: {
        title: "",
        body: ""
      },
      weather: {
        location: "",
        status: "",
        temp: "",
        pressure: "",
        humidity: ""
      }
    };
  },
  created() {
    this.updateWeather();
    if (this.formState.formBInput <= this.formState.formAInput) {
      this.message.title = "Success🤝";
      this.message.body = "You have reached an agreement.";
    } else {
      this.message.title = "Negotiation Failed 😞";
      this.message.body = "You were unable to reach an agreement. Try again.";
    }
  },
  methods: {
    updateWeather() {
      getWeather()
        .then(({ data }) => {
          this.weather.location = data.name;
          this.weather.temp = (data.main.temp - 273).toFixed(0);
          this.weather.pressure = data.main.pressure;
          this.weather.humidity = data.main.humidity;
          this.weather.status = data.weather[0].description;
        })
        .catch(err => console.log(err));
    },
    closeModal() {
      this.showModal = false;
      this.$emit("reset", "");
    }
  }
};
</script>

<style>
.modal {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100vw;
  overflow-x: hidden;
  overflow-y: hidden;
}
.modal__button {
  margin-top: 2rem;
  appearance: none;
  outline: none;
  border: none;
  background: none;
  cursor: pointer;
  display: inline-block;
  padding: 1rem 2.5rem;
  background-image: linear-gradient(141deg, #1fc8db 0%, #2cb5e8 75%);
  border-radius: 0.5rem;
  color: #fff;
  font-weight: 700;
  font-size: 1.8rem;
  box-shadow: 0 2px 2px 0 rgba(0, 0, 0, 0.14),
    0 3px 1px -2px rgba(0, 0, 0, 0.12), 0 1px 5px 0 rgba(0, 0, 0, 0.2);
  transition: 0.4s ease-out;
}

.modal__button:hover {
  box-shadow: 0 2px 2px 0 rgba(0, 0, 0, 0.14),
    0 3px 1px -2px rgba(0, 0, 0, 0.12), 0 1px 5px 0 rgba(0, 0, 0, 0.2);
}
.modal__overlay {
  position: absolute;
  width: -100rem;
  height: -100rem;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 100;
  background: rgba(0, 0, 0, 0.7);
}
.modal__content p:first-of-type {
  font-weight: 700;
  margin-top: 1rem;
}
.modal__content {
  text-align: center;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 102;
  width: 100%;
  max-width: 40rem;
  background-color: #fff;
  border-radius: 2rem;
  padding: 2.5rem;
}

/* Fade Effect */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
/* Slide Effect */
.slide-enter-active,
.slide-leave-active {
  transition: transform 0.5s;
}
.slide-enter,
.slide-leave-to {
  transform: translateY(-50%) translateX(100vw);
}
</style>