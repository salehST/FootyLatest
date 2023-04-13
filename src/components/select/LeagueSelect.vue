<template>
  <div>
    <DropdownButton class="league-select" :buttonText="buttonText">
      <template v-slot:default="{ close }">
        <h1 class="mb-12 mt-1 text-2xl label-h">{{ $t('standings.select_league') }} / finals</h1>
        <OptionList
          v-if="options.length"
          :options="options"
          v-model="selection"
          v-on:input="close"
        />
        <div v-else-if="selectedTeam" class="flex justify-center w-full">
          <span class="text-lg text-gray-700 text-center p-6"
            >{{ selectedTeam.name }} have not been entered into any leagues or finals yet</span
          >
        </div>
      </template>
    </DropdownButton>
  </div>
</template>

<script lang="ts">
import DropdownButton from '@/layouts/common/DropdownButton.vue';
import OptionList from '@/layouts/common/OptionList.vue';
import Vue from 'vue';
import { mapGetters } from 'vuex';
export default Vue.extend({
  components: {
    DropdownButton,
    OptionList,
  },
  data: () => ({
    options: [],
  }),
  watch: {
    options: {
      handler(newVal) {
        if (!newVal || !newVal.length) {
          this.selection = null;
        }
      },
    },
    selectedTeam: {
      immediate: true,
      async handler(newVal) {
        if (!newVal) return [];
        const teamId = newVal.id;
        const route = this.$route.name;
        console.log("route", route)
        switch (route) {
          case 'clubhouse':
            const fixtureLeagueDatas = await this.$store.dispatch('getFixtureLeagues', teamId);
            const fixture = (this as any).leagueFilter(fixtureLeagueDatas);
            this.options = fixture && fixture.length ? fixture : [];
            break;
          case 'fixtures':
            const fixtureLeagueData = await this.$store.dispatch('getFixtureLeagues', teamId);
            const fixtures = (this as any).leagueFilter(fixtureLeagueData);
            this.options = fixtures && fixtures.length ? fixtures : [];
            break;
          case 'bookings':
            const fixtureLeagueDataBooking = await this.$store.dispatch(
              'getFixtureLeagues',
              teamId
            );
            const bookings = (this as any).leagueFilter(fixtureLeagueDataBooking);
            this.options = bookings && bookings.length ? bookings : [];
            break;
          case 'pick-location':
            const fixtureLeagueDataPickLocation = await this.$store.dispatch(
              'getFixtureLeagues',
              teamId
            );
            const pickLocation = (this as any).leagueFilter(fixtureLeagueDataPickLocation);
            this.options = pickLocation && pickLocation.length ? pickLocation : [];
            break;
          case 'book-field':
            const fixtureLeagueDataBookField = await this.$store.dispatch(
              'getFixtureLeagues',
              teamId
            );
            const bookField = (this as any).leagueFilter(fixtureLeagueDataBookField);
            this.options = bookField && bookField.length ? bookField : [];
            break;
          case 'booking-payment':
            const fixtureLeagueDataBookPayment = await this.$store.dispatch(
              'getFixtureLeagues',
              teamId
            );
            const bookPayment = (this as any).leagueFilter(fixtureLeagueDataBookPayment);
            this.options = bookPayment && bookPayment.length ? bookPayment : [];
            break;
          case 'results':
            const resultLeagueData = await this.$store.dispatch('getResultLeagues', teamId);
            const results = (this as any).leagueFilter(resultLeagueData);
            this.options = results && results.length ? results : [];
            break;
          case 'standings':
            const standingLeagueData = await this.$store.dispatch('getStandingLeagues', teamId);
            console.log('standing leagues: ', standingLeagueData);
            const standings = (this as any).leagueFilter(standingLeagueData);
            console.log('after filter: ', standings);
            this.options = standings && standings.length ? standings : [];
            break;
          default:
            return (this.options = []);
        }
      },
    },
  },
  computed: {
    ...mapGetters([
      'getSelectedFixtureLeague',
      'getSelectedResultLeague',
      'getSelectedStandingLeague',
      'getSelectedFixtureTeam',
      'getSelectedResultTeam',
      'getSelectedStandingTeam',
    ]),
    selectedTeam() {
      const route = this.$route.name;
      console.log("route", route)
      switch (route) {
        case 'clubhouse':
          return this.getSelectedFixtureTeam;
        case 'fixtures':
          return this.getSelectedFixtureTeam;
        case 'results':
          return this.getSelectedResultTeam;
        case 'standings':
          return this.getSelectedStandingTeam;
        case 'bookings':
          return this.getSelectedFixtureTeam;
        case 'pick-location':
          return this.getSelectedFixtureTeam;
        case 'book-field':
          return this.getSelectedFixtureTeam;
        case 'booking-payment':
          return this.getSelectedFixtureTeam;
        default:
          return '';
      }
    },
    selection: {
      get(): any {
        const route = this.$route.name;
        console.log("route", route)
        switch (route) {
          case 'clubhouse':
            return this.getSelectedFixtureLeague;
          case 'fixtures':
            return this.getSelectedFixtureLeague;
          case 'results':
            return this.getSelectedResultLeague;
          case 'standings':
            return this.getSelectedStandingLeague;
          case 'bookings':
            return this.getSelectedFixtureLeague;
          case 'pick-location':
            return this.getSelectedFixtureLeague;
          case 'book-field':
            return this.getSelectedFixtureLeague;
          case 'booking-payment':
            return this.getSelectedFixtureLeague;
          default:
            return '';
        }
      },

      set(value: any): any {
        const route = this.$route.name;
        switch (route) {
          case 'clubhouse':
            value &&
              value.type &&
              this.$store.commit(
                'setIsFinalsSelected',
                value && value.type && value.type === 'leagues' ? false : true
              );
            return this.$store.commit('setFixtureLeague', value);
          case 'bookings':
            value &&
              value.type &&
              this.$store.commit(
                'setIsFinalsSelected',
                value && value.type && value.type === 'leagues' ? false : true
              );
            return this.$store.commit('setFixtureLeague', value);
          case 'fixtures':
            value &&
              value.type &&
              this.$store.commit(
                'setIsFinalsSelected',
                value && value.type && value.type === 'leagues' ? false : true
              );
            return this.$store.commit('setFixtureLeague', value);
          case 'results':
            value &&
              value.type &&
              this.$store.commit(
                'setIsResultFinalsSelected',
                value && value.type && value.type === 'leagues' ? false : true
              );
            return this.$store.commit('setResultLeague', value);
          case 'standings':
            value &&
              value.type &&
              this.$store.commit(
                'setIsStandingsFinalsSelected',
                value && value.type && value.type === 'leagues' ? false : true
              );
            return this.$store.commit('setStandingLeague', value);
          default:
            return '';
        }
      },
    },
    buttonText() {
      // @ts-ignore
      return (this as any).selection && this.selection.name
        ? (this as any).selection.name
        : this.$t('standings.select_league');
    },
  },
  methods: {
    leagueFilter(leagues: any) {
      return leagues
        .sort((a: any, b: any) => {
          return new Date(b.kick_off_date).getTime() - new Date(a.kick_off_date).getTime();
        })
        .sort((a: any, b: any) => {
          const aName = a.status;
          const bName = b.status;
          if (aName > bName) return -1;
          if (aName < bName) return 1;
          return 0;
        });
    },
  },
});
</script>
