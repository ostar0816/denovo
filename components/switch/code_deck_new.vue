<template>
	<div class="container-fluid">
		<div class="row">
			<div class="wrapper single">
				<time-display></time-display>
				<ol class="breadcrumb">
					<li class="breadcrumb-item">
						<a href="#">Switch</a>
					</li>
					<li class="breadcrumb-item">
						<router-link to="/switch/code_deck">Code Deck </router-link>
					</li>
					<li class="breadcrumb-item active">New</li>
				</ol>
				<h1 class="page-header">Create Code Deck</h1>
				<router-link to="/switch/code_deck" class="btn btn_white back">
					<span class="dnl_icon dnl_import"></span> Back </router-link>
				<div class="white_pad">
					<div class="select_group row">
						<div class="col-md-8 col-sm-8">
							<form class="form" @submit.prevent="validateBeforeSubmit">
								<div class="row">
									<div class="form-group">
										<div class="col-sm-6">
											<label for="groupname">Code Deck Name:</label>
											<input type="text" name="name" v-model="name" class="form-control" v-validate="'required'" placeholder="Enter Name">
											<span v-show="errors.has('name')" class="help is-danger">{{ errors.first('name') }}</span>
										</div>
									</div>
									<div class="clearfix little-space"></div>
								</div>
								<div class="button_set little-space">
									<button class="btn btn-primary" type="submit">
										Submit
									</button>
									<i v-show="loading" class="fa fa-spinner fa-spin" style="font-size:20px;line-height:40px;"></i>
								</div>
							</form>
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>   
const api = require("../../api");
const auth = require("../../auth");
const vSpinner = require('vue!../main_components/fadeloader.vue');
import {mapGetters,	mapActions} from 'vuex'

module.exports = {
	created: function () {
	},
	data: function () {
		return {
			requestForm: {},
		}
	},
	watch: {

	},
	mounted: function () {

	},
	methods: {
		...mapActions({
			setAsyncConfirm: 'async_confirm/setAsyncConfirm',
			setMessage: 'app_message/setAppMessage'
		}),
		validateBeforeSubmit() {
			this.$validator.validateAll().then(() => {

				// eslint-disable-next-line
				this.loading = true;
				var requestForm = {};
				requestForm.name = this.name;
				requestForm.is_virtual = this.is_virtual;
				this.$http.post(api.getEndpointUrl() + '/v1/switch/code_deck/create',
					requestForm,
					{
						headers: {
							"X-Auth-Token": auth.getToken()
						}
					}
				).then(
					function (response) {
						this.loading = false;
						console.log(response);

						this.$router.push('/switch/code_deck');
						this.setMessage('The code deck has been created!');
					},
					function (response) {
						this.loading = false;
						console.log(response);

						this.setMessage({
							message: "Creating code deck is failed!",
							type: "error"
						});
					}
					)

			}).catch(() => {
				// eslint-disable-next-line

			});
		}
	}
}
</script>

<style>
	.wrapper.single .white_pad {
		margin-bottom: 20px;
	}
	
	.little-space {
		margin: 15px 0;
	}
	
	.ms-container .ms-selectable,
	.ms-container .ms-selection {
		width: 47%;
	}
	
	.button_set.little-space {
		margin-top: 25px;
	}
	
	.button_set.little-space .btn {
		width: 23%;
	}
	
	.button_set.little-space .btn-primary {
		margin-right: 2px;
	}
	
	.btn_white.back {
		color: #222;
		background: #f7f8f9;
		border: 1px solid #e2e6ed;
		box-shadow: 0px 1px 1px 0px rgba(227, 230, 237, 0.74);
		padding: 9px 30px;
		margin-bottom: 20px;
	}
	
	.btn_white.back:hover {
		background: #fff;
	}
	
	.btn_white.back .dnl_icon {
		color: #03c0fc;
		font-size: 16px;
		vertical-align: middle;
		margin: -2px 5px 0 -10px;
		display: inline-block;
		transform: rotate(-90deg);
		font-weight: 900;
	}
	.daysselect {
		border:1px solid #dee0e3;
		padding:8px;
		border-radius: 4px;
	}
	@media (max-width: 480px) {
		.white_pad.select_group {
			width: 100%;
		}
		.clearfix.little-space,
		.button_set.little-space {
			margin: 0;
		}
	}
	
	@media (min-width: 319px) and (max-width: 479px) {
		.ms-container .ms-selectable,
		.ms-container .ms-selection {
			width: 100%;
			margin-bottom: 20px;
		}
		.ms-container {
			background-position: 50% 48%;
		}
		.ms-selection {
			margin-top: 20px;
		}
	}

</style>