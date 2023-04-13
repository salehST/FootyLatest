<template>
  <BaseLayout :pageTitle="$t('global.bookings')" :headerPull="'lg'" maxWidth="996px">
    <!-- <template slot="header">
      <TeamSelect />
      <LeagueSelect />
    </template> -->
    <template slot="content">
      <div class="flex mb-32 items-start flex-col sm:flex-row justify-start sm:justify-between">
        <h2 class="uppercase font-bold text-28 text-white mb-6 md:mb-0">Upcoming Booking</h2>
        <div class="btn-area flex flex-row gap-4 justify-between items-center w-full md:w-auto">
          <router-link
            :to="`/pick-location/${countryid}?`"
            class="org-btn mr-0 md:mr-4 w-full md:w-auto justify-center flex"
            >Book a field</router-link
          >
        </div>
      </div>
      <PreviousBooking :dates="upcommingDate" class="mb-7" />
      <div class="mb-10">
        <h2 class="uppercase font-bold text-28 text-whitee md:mb-0">Previous Bookings</h2>
      </div>
      <PreviousBooking :dates="previousDates" class="mb-32" />
    </template>
  </BaseLayout>
</template>
<script lang="ts">
import Vue from 'vue';
import { mapGetters } from 'vuex';
import BaseLayout from '@/layouts/BaseLayout.vue';
import PreviousBooking from '@/components/booking/PreviousBooking.vue';
// import TeamSelect from '@/components/select/TeamSelect.vue';
// import LeagueSelect from '@/components/select/LeagueSelect.vue';
export default Vue.extend({
  components: {
    BaseLayout,
    PreviousBooking,
    // TeamSelect,
    // LeagueSelect,
  },
  computed: {
    ...mapGetters(['getplayerBookings', 'getPlayer']),
    countryid() {
      let value;
      if ((this as any).getPlayer) value = (this as any).getPlayer.city.country.id;
      return value;
    },
    upcommingDate() {
      const currentDate = new Date(); // This will give you the current date and time
      const today = currentDate.toISOString().slice(0, 10); // This will give you the date in the YYYY-MM-DD format
      const upcomingDates: any = (this as any).getplayerBookings
        .filter((ele: any) => ele.date > today)
        .sort((a: any, b: any) => {
          const dateA: any = new Date(a.date);
          const dateB: any = new Date(b.date);
          return dateA - dateB;
        });
      return [{ ...upcomingDates[0], disable: false }];
    },
    previousDates() {
      const currentDate = new Date(); // This will give you the current date and time
      const today = currentDate.toISOString().slice(0, 10); // This will give you the date in the YYYY-MM-DD format
      let previousDates: any = (this as any).getplayerBookings
        .filter((ele: any) => ele.date < today)
        .sort((a: any, b: any) => {
          const dateA: any = new Date(a.date);
          const dateB: any = new Date(b.date);
          return dateB - dateA;
        });
      previousDates = previousDates.map((ele: any) => {
        return { ...ele, disable: true };
      });
      console.log('previousDates', previousDates);
      return previousDates;
    },
  },
  async created() {
    console.log('bookings', this.getPlayer);
  },
});
</script>
