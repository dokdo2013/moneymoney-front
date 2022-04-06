<script>
	// 일자, 지출항목분류 (식비, 문화비, 기타교통비 (차량), 기타지출비, 예비비충당 (토스뱅크), 기타), 항목, 항목상세, 
	// 결제수단 (토스뱅크카드, 케이뱅크카드, 부천페이, 카카오뱅크체크, 하나카드, 신한딥드림, 신한카카오페이, 부산썸뱅크, 국민체크7157, 현금결제)
	// 결제금액, 비고

	import { FormGroup, Input, Button } from 'sveltestrap';
	import { Pincode, PincodeInput} from 'svelte-pincode';
	import moment from 'moment';
	import Swal from 'sweetalert2';

	let data_date = moment().format('YYYY-MM-DD');
	let data_category = '1';
	let data_pay_method = '1';
	let data_item = '';
	let data_item_detail = '';
	let data_price = 0;
	let data_etc = '';
	let code = [];
  let value = "";

	const handleSubmit = () => {
		let validFlag = true;
		let validMessage = '';

		if (data_price <= 0) {
			Swal.fire({
				icon: 'error',
				title: '등록 실패',
				text: '금액을 0원 이상으로 입력해주세요.'
			});
			return false;
		}

		const formDto = {
			date: data_date,
			category: parseInt(data_category),
			payMethod: parseInt(data_pay_method),
			item: data_item,
			itemDetail: data_item_detail,
			price: data_price,
			etc: data_etc,
			otpCode: value
		};
		console.log(formDto);
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
					<option value="1">식비</option>
					<option value="2">문화비</option>
					<option value="3">기타교통비 (차량)</option>
					<option value="4">기타지출비</option>
					<option value="5">예비비충당 (토스뱅크)</option>
					<option value="6">기타</option>
				</Input>
			</FormGroup>
			<FormGroup floating label="결제수단">
				<Input type="select" bind:value={data_pay_method}>
					<option value="1">토스뱅크카드</option>
					<option value="2">케이뱅크카드</option>
					<option value="3">부천페이</option>
					<option value="4">카카오뱅크체크</option>
					<option value="5">하나카드</option>
					<option value="6">신한딥드림</option>
					<option value="7">신한카카오페이</option>
					<option value="8">부산썸뱅크</option>
					<option value="9">국민체크7157</option>
					<option value="10">현금결제</option>
				</Input>
			</FormGroup>
			<FormGroup floating label="항목" bind:value={data_item}>
				<Input type="text"></Input>
			</FormGroup>
			<FormGroup floating label="항목 상세" bind:value={data_item_detail}>
				<Input type="text"></Input>
			</FormGroup>
			<FormGroup floating label="비고" bind:value={data_etc}>
				<Input type="text"></Input>
			</FormGroup>
			<FormGroup floating label="결제금액 (₩)" bind:value={data_price}>
				<Input type="number"></Input>
				<div style="margin-top: 5px; text-align: right">
					<Button size="sm">+ 100,000</Button>
					<Button size="sm">+ 10,000</Button>
					<Button size="sm">+ 1,000</Button>
					<Button size="sm">+ 100</Button>
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
			<div style="margin-top: 30px">
				<Button block>등록</Button>
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

	.menu-button {
		background-color: #333;
		border: 0px;
		padding: 4px 16px ;
		color: white;

	}

	.menu-button.selected {
		background-color: rgba(170,40,255, 0.7);
		
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