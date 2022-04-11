<script>
	// 일자, 지출항목분류 (식비, 문화비, 기타교통비 (차량), 기타지출비, 예비비충당 (토스뱅크), 기타), 항목, 항목상세, 
	// 결제수단 (토스뱅크카드, 케이뱅크카드, 부천페이, 카카오뱅크체크, 하나카드, 신한딥드림, 신한카카오페이, 부산썸뱅크, 국민체크7157, 현금결제)
	// 결제금액, 비고

	import { FormGroup, Input, Button, Spinner } from 'sveltestrap';
	import { Pincode, PincodeInput} from 'svelte-pincode';
	import moment from 'moment';
	import axios from 'axios';
	import Swal from 'sweetalert2';

	let data_date = moment().format('YYYY-MM-DD');
	let data_category = '식비';
	let data_pay_method = '토스뱅크카드';
	let data_item = '';
	let data_item_detail = '';
	let data_price = 0;
	let data_etc = '';
	let code = [];
  let value = "";
	let is_loading = false;

	const reset = () => {
		data_date = moment().format('YYYY-MM-DD');
		data_category = '식비';
		data_pay_method = '토스뱅크카드';
		data_item = '';
		data_item_detail = '';
		data_price = 0;
		data_etc = '';
		code = [];
		value = "";
	}

	const handleSubmit = () => {
		is_loading = true;
		let validFlag = true;
		let validMessage = '';

		if (data_price <= 0) {
			Swal.fire({
				icon: 'error',
				title: '등록 실패',
				text: '금액을 0원 이상으로 입력해주세요.'
			});
			is_loading = false;
			return false;
		}

		const formDto = {
			date: data_date,
			category: data_category,
			payMethod: data_pay_method,
			item: data_item,
			itemDetail: data_item_detail,
			price: data_price,
			etc: data_etc,
			otpCode: value
		};
                let api_url = '';
                if (window.location.hostname === 'dev-money.haenu.com') {
		        api_url = 'https://pr5wq459k5.execute-api.ap-northeast-2.amazonaws.com/dev/';
		} else { 
                        api_url = 'https://v4bti9c0t3.execute-api.ap-northeast-2.amazonaws.com/v1/';
		}
                axios.post(api_url + 'money', formDto)
			.then(response => {
				if (response.status === 200) {
					Swal.fire({
						icon: 'success',
						title: '등록 성공',
						text: '지출내역 등록에 성공했어요.'
					});
					reset();
				} else {
					Swal.fire({
						icon: 'error',
						title: '등록 실패',
						text: 'API 요청에는 성공했으나 등록에는 실패했어요.'
					});
				}
				console.log(response);
			})
			.catch(error => {
				if (error.response) {
					if (error.response.status === 400) {
						Swal.fire({
							icon: 'error',
							title: '등록 실패',
							text: '오류가 발생하여 등록에 실패했어요. (400)'
						});
					} else if (error.response.status === 401) {
						Swal.fire({
							icon: 'error',
							title: '등록 실패',
							text: 'OTP 인증에 실패했어요. (401)'
						});
					} else if (error.response.status === 404) {
						Swal.fire({
							icon: 'error',
							title: '등록 실패',
							text: 'API 주소가 잘못되어 등록에 실패했어요. (404)'
						});
					} else if (error.response.status === 422) {
						Swal.fire({
							icon: 'error',
							title: '등록 실패',
							text: '요청 정보 전달이 잘못되어 등록에 실패했어요. (422)'
						});
					} else if (error.response.status === 429) {
						Swal.fire({
							icon: 'error',
							title: '등록 실패',
							text: '이용자가 많아 현재 API에 접속할 수 없어요. (429)'
						});
					} else if (error.response.status === 500) {
						Swal.fire({
							icon: 'error',
							title: '등록 실패',
							text: '알 수 없는 오류로 API 요청에 실패했어요. (500)'
						});
					} else if (error.response.status === 502) {
						Swal.fire({
							icon: 'error',
							title: '등록 실패',
							text: '현재 API 접속이 원활하지 않아요. (502)'
						});
					} else {
						Swal.fire({
							icon: 'error',
							title: '등록 실패',
							text: '알 수 없는 오류로 API 요청에 실패했어요. 자세한 내용은 콘솔을 확인해주세요. (etc)'
						});
					}
					console.log(error.response);
				} else {
					Swal.fire({
							icon: 'error',
							title: '등록 실패',
							text: '알 수 없는 오류로 API 요청에 실패했어요. Lambda 로그를 확인해주세요. (etc)'
						});
				}
			})
			.finally(() => {
				is_loading = false;
			});
	}

	const addMoney = add => {
		data_price = data_price + add;
	}
</script>

<main>
	<div id="container">
		<h1 class="text-center">내 <span class="title-impact">머니머니</span> 등록</h1>
		<form on:submit|preventDefault={handleSubmit}>
			<FormGroup floating label="일자">
				<Input type="date" bind:value={data_date}></Input>
			</FormGroup>
			<FormGroup floating label="지출분류">
				<Input type="select" bind:value={data_category}>
					<option value="식비" selected={data_category === "식비"}>식비</option>
					<option value="문화비" selected={data_category === "문화비"}>문화비</option>
					<option value="기타교통비 (차량)" selected={data_category === "기타교통비 (차량)"}>기타교통비 (차량)</option>
					<option value="기타지출비" selected={data_category === "기타지출비"}>기타지출비</option>
					<option value="예비비충당 (토스뱅크)" selected={data_category === "예비비충당 (토스뱅크)"}>예비비충당 (토스뱅크)</option>
					<option value="기타" selected={data_category === "기타"}>기타</option>
				</Input>
			</FormGroup>
			<FormGroup floating label="결제수단">
				<Input type="select" bind:value={data_pay_method}>
					<option value="토스뱅크카드" selected={data_pay_method === "토스뱅크카드"}>토스뱅크카드</option>
					<option value="케이뱅크카드" selected={data_pay_method === "케이뱅크카드"}>케이뱅크카드</option>
					<option value="부천페이" selected={data_pay_method === "부천페이"}>부천페이</option>
					<option value="카카오뱅크체크" selected={data_pay_method === "카카오뱅크체크"}>카카오뱅크체크</option>
					<option value="하나카드" selected={data_pay_method === "하나카드"}>하나카드</option>
					<option value="신한딥드림" selected={data_pay_method === "신한딥드림"}>신한딥드림</option>
					<option value="신한카카오페이" selected={data_pay_method === "신한카카오페이"}>신한카카오페이</option>
					<option value="부산썸뱅크" selected={data_pay_method === "부산썸뱅크"}>부산썸뱅크</option>
					<option value="국민체크7157" selected={data_pay_method === "국민체크7157"}>국민체크7157</option>
					<option value="현금결제" selected={data_pay_method === "현금결제"}>현금결제</option>
				</Input>
			</FormGroup>
			<FormGroup floating label="항목">
				<Input type="text" bind:value={data_item}></Input>
			</FormGroup>
			<FormGroup floating label="항목 상세">
				<Input type="text" bind:value={data_item_detail}></Input>
			</FormGroup>
			<FormGroup floating label="비고">
				<Input type="text" bind:value={data_etc}></Input>
			</FormGroup>
			<FormGroup floating label="결제금액 (₩)">
				<Input type="number" bind:value={data_price}></Input>
				<div style="margin-top: 5px; text-align: right">
					<Button size="sm" on:click={e => {e.preventDefault(); addMoney(100000);}}>+ 100,000</Button>
					<Button size="sm" on:click={e => {e.preventDefault(); addMoney(10000);}}>+ 10,000</Button>
					<Button size="sm" on:click={e => {e.preventDefault(); addMoney(1000);}}>+ 1,000</Button>
					<Button size="sm" on:click={e => {e.preventDefault(); addMoney(100);}}>+ 100</Button>
				</div>
			</FormGroup>
			<div class="text-center">
				<label class="label" style="color: white" for="pin">OTP Code</label>
			</div>
			<div style="display: flex; justify-content: center">
				<Pincode id="pin" type="numeric" bind:code bind:value>
					<PincodeInput />
					<PincodeInput />
					<PincodeInput />
					<PincodeInput />
					<PincodeInput />
					<PincodeInput />
				</Pincode>
			</div>
			<div style="margin: 30px 0">
				<Button disabled={is_loading} block>
					{#if is_loading}
						<Spinner size="sm" />
					{/if}
					등록</Button>
			</div>
		</form>
	</div>
</main>

<style>
	:global(body) {
		background-color: #0f2020 !important;
		color: #bfc2c7;
	}
	:global(.swal2-popup) {
		/* font-size: 1.6rem !important; */
		font-family: 'IBM Plex Sans KR', sans-serif !important;
	}

/** Pincode **/
:global([data-pincode]) {
  display: inline-flex;
  border: 1px solid #e0e0e0;
}

/** PincodeInput */
:global([data-pincode] input) {
  max-width: 3rem;
  padding: 0.5rem 1rem;
  margin: 0;
  border: 0;
  border-radius: 0;
  text-align: center;
}

:global([data-pincode] input:focus) {
  z-index: 1;
}

:global([data-pincode] input:not(:last-of-type)) {
  border-right: 1px solid #e0e0e0;
}

:global(input[type="number"]::-webkit-outer-spin-button,
input[type="number"]::-webkit-inner-spin-button)
 {
    -webkit-appearance: none;
    margin: 0;
}
	main {
		margin: 0;
		font-family: 'IBM Plex Sans KR', sans-serif;
	}
	.title-impact {
		color: #c56fff;
		font-weight: 900;
	}

	#container {
		max-width: 500px;
		margin: 0 auto;
	}

	h1 {
		color: rgb(215, 155, 255);
		text-transform: uppercase;
		font-size: 32px;
		font-weight: 600;
		margin: 30px 0;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
