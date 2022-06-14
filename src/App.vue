<template>
  <div class="container-fluid mt-5">
    <div class="row">
      <div class="">
        <keep-alive>
          <transition name="fade" mode="out-in" appear>
            <component :is="activeView" />
          </transition>
        </keep-alive>
      </div>
    </div>
    <div class="row mt-3">
      <div class="col-xl-4 col-md-12">
        <CalendarEntry />
      </div>
      <div class="col-xl-2 col-md-12 mt-2">
        <div class="float-end">
          <button
            class="btn btn-lg mb-2"
            :class="buttonSettingsClasses"
            @click="toggleDisplaySettings()"
          >
            <i class="fas fa-cogs"></i>
          </button>
        </div>

        <transition
          enter-active-class="animate__animated animate__bounceInRight"
          leave-active-class="animate__animated animate__bounceOutRight"
        >
          <CalendarSettings v-if="displaySettings" />
        </transition>
      </div>
    </div>
  </div>
</template>

<script>
import { defineAsyncComponent } from "vue";
import Store from "./store";
import CalendarWeek from "./components/CalendarWeek";
import CalendarWeekAsList from "./components/CalendarWeekAsList";
import CalendarEntry from "./components/CalendarEntry";

export default {
  name: "App",
  components: {
    CalendarWeek,
    CalendarWeekAsList,
    CalendarEntry,
    CalendarSettings: defineAsyncComponent(() =>
      import(
        /*webpackChunkName: 'CalendarSettingsComponent*/ "./components/CalendarSettings.vue"
      )
    ),
  },
  data() {
    return {
      displaySettings: true,
    };
  },
  computed: {
    buttonSettingsClasses() {
      return this.displaySettings ? ["btn-success"] : ["btn-outline-success"];
    },
    activeView() {
      return Store.getters.activeView();
    },
  },
  methods: {
    toggleDisplaySettings() {
      this.displaySettings = !this.displaySettings;
    },
  },
};
</script>

<style>

@import "~@fortawesome/fontawesome-free/css/all.min.css";
@import "~animate.css/animate.min.css";

@import "~bootstrap/dist/css/bootstrap.min.css";


.square {
  width: 40px;
  height: 40px;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.fade-enter-to,
.fade-leave-from {
  opacity: 1;
}

.fade-leave-active,
.fade-enter-active {
  transition: all 400ms ease-out;
}
</style>
