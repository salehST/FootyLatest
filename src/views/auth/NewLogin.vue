	<template>
  <BaseLayout :pageTitle="$t('global.bookings')" :headerPull="'lg'">
    <template slot="content">
	<BookingLayout :step="step" :page-title="pageTitle">
		<div class="content-text mb-85" style="background: #ffffff; padding: 10px"></div>
	</BookingLayout>
	</template>
	</BaseLayout>
</template>
<script>
import BookingLayout from '@/layouts/booking/BookingLayout.vue';
import BaseLayout from '@/layouts/BaseLayout.vue';
export default {
	components: {
		BookingLayout,
		BaseLayout
	},
	data() {
		return {
			step: 3,
			pageTitle: this.$t('booking.log_in'),
		};
	},
};
</script>
<style></style>
