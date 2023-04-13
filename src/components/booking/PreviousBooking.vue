<template>
  <div v-if="dates">
    <div class="bg-white w-full p-4 rounded-md shadow mb-7" v-for="(date, i) in dates" :key="i">
      <div class="inner-info flex items-start flex-col md:flex-row justify-between gap-4 w-full">
        <div class="img hidden md:block w-52 h-36 min-w-max overflow-hidden rounded">
          <img :src="date.pitch.location.logo_url" class="object-cover w-full h-full" alt="" />
        </div>
        <div class="text-area w-full">
          <h3
            @click="toggleDiv(1)"
            class="text-brand text-2xl font-700 mb-0 flex w-full items-center justify-between mb-2"
          >
            <span class="flex-wrap w-2/3">{{ date.pitch.location.name }}</span>
            <button :class="[activeClass, { activeBtn: showDiv === 1 }]">
              <img
                src="/img/icons/icon-arrow.svg"
                class="h-32 w-32 flex md:hidden"
                alt="arrow-icon"
              />
            </button>
          </h3>
          <div class="booking-more-info overflow-hidden mt-3 md:mt-0" v-show="showDiv === 1">
            <!-- <p class=" text-brand  text-18 md:text-20 font-semibold mb-3">{{ date.display_date }} on Field {{ date.pitch.name }}</p> -->
            <ul>
              <li class="flex items-center gap-2 justify-start text-brand text-base mb-2">
                <img src="/img/icons/location.png" class="h-5 w-5" alt="icon" />
                <span class="mx-2">{{ date.pitch.location.address }}</span>
              </li>
              <li class="flex items-center gap-2 justify-start text-brand text-base mb-2">
                <img src="/img/icons/field.png" class="h-5 w-5" alt="icon" />
                <span class="mx-2">
                  {{ date.pitch.type.replace('v', ' vs. ') }}, field {{ date.pitch.name }}</span
                >
              </li>
              <li class="flex items-center gap-2 justify-start text-brand text-base">
                <img src="/img/icons/time.png" class="h-5 w-5" alt="icon" />
                <span class="mx-2"> {{ date.display_date }}</span>
              </li>
            </ul>
            <div
            v-if="date.disable"
              class="flex md:hidden w-full min-w-max lg:w-1/4 mt-6 text-right justify-end"
            >
              <span href="" class="disabled-button">Edit booking</span>
            </div>
			<div
            v-else
			class="btn-a flex md:hidden w-full min-w-max lg:w-1/4 mt-6 text-right justify-end"
		  >
			<a @click="editBooking" class="org-btn-2">Edit booking</a>
		  </div>
            
          </div>
        </div>
        <div
		v-if="date.disable"
          class="
            hidden
            md:flex
            w-full
            min-w-max
            lg:w-1/4
            text-right
            justify-start
            md:justify-end
          "
        >
          <span class="disabled-button">Edit booking</span>
        </div>
		<div
	v-else
          class="
            btn-a
            hidden
            md:flex
            w-full
            min-w-max
            lg:w-1/4
            text-right
            justify-start
            md:justify-end
          "
        >
          <a @click="editBooking" class="org-btn-2">Edit booking</a>
        </div>
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  data() {
    return {
      showDiv: 1 as number | null,
      isMobile: false,
    };
  },
  props: {
    dates: {
      required: true,
      type: Array,
    },
  },

  computed: {
    activeClass(): string {
      return (this as any).showDiv !== null ? 'active' : '';
    },
  },
  methods: {
	editBooking() {
			console.log('datas: ');
			// this.$router.push({ name: 'edit-booking' });
		},
    toggleDiv(divNumber: number): void {
      if ((this as any).showDiv === divNumber) {
        (this as any).showDiv = null;
      } else {
        (this as any).showDiv = divNumber as number | null;
      }
    },
  },
});
</script>
<style scoped>
.text-area img {
  transform: rotate(0deg);
  transition: 300ms;
}
.active.activeBtn img {
  transform: rotate(180deg);
}
.disabled-button{
	color: #fff;
    padding: 10px 16px;
    border-radius: 5px;
    font-weight: 700;
    transition: 300ms;
    background: #DDD8D8;
}
@media only screen and (min-width: 767.99px) {
  .booking-more-info {
    display: block !important;
  }
}
</style>
