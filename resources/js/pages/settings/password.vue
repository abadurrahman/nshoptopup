<template>
<div class="container mx-auto mt-2 md:mt-8" style="min-height: 460px;">
	<div class="justify-center shadow-lg hover:shadow-xl m-auto w-full md:w-6/12 xl:w-6/12 p-2 md:p-4 m-2 md:m-0" style="margin: auto">
		<h4 class="text-center font-bold text-gray-900">{{ $t('your_password') }}</h4>
		<form @submit.prevent="update" @keydown="form.onKeydown($event)">

			<alert-success class="bg-orange-700 text-white text-center" :form="form" :message="$t('password_updated')" />
			<!-- Password -->
			<div class="form-group row">
				<label class="col-md-3 col-form-label text-md-right font-bold">{{ $t('new_password') }}</label>
				<div class="col-md-7">
					<input v-model="form.password" :class="{ 'is-invalid': form.errors.has('password') }" class="appearance-none block w-full bg-gray-200 text-gray-700 border border-gray-200 rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500" placeholder="New Password" type="password" name="password">
					<has-error class="text-pink-700" :form="form" field="password" />
				</div>
			</div>

			<!-- Password Confirmation -->
			<div class="form-group row py-5">
				<label class="col-md-3 col-form-label text-md-right font-bold">{{ $t('confirm_password') }}</label>
				<div class="col-md-7">
					<input v-model="form.password_confirmation" :class="{ 'is-invalid': form.errors.has('password_confirmation') }" class="appearance-none block w-full bg-gray-200 text-gray-700 border border-gray-200 rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500" placeholder="Confirm Password" type="password" name="password_confirmation">
					<has-error :form="form" field="password_confirmation" />
				</div>
			</div>

			<!-- Submit Button -->
			<div class="form-group row">
				<div class="col-md-9 ml-md-auto">
					<button :loading="form.busy" type="success" class="bg-orange-500 hover:bg-orange-700 text-white font-bold py-2 px-4 rounded mt-3">
						{{ $t('update') }}
					</button>
				</div>
			</div>

		</form>
	</div>
</div>
</template>

<script>
import Form from 'vform';
export default {
	scrollToTop: false,
	metaInfo () {
		return { title: this.$t('settings') }
	},

	data: () => ({
		form: new Form({
			password: '',
			password_confirmation: ''
		})
	}),

	methods: {
		async update () {
			await this.form.patch('/api/settings/password')
			this.form.reset()
		}
	}
}
</script>
<style scoped>
.is-invalid{
	border: 2px solid red;
}
input{
		width: 100% !important;
		height: 40px;
}
.card{
	margin: auto;
}
</style>