<script>
	// 일자, 지출항목분류 (식비, 문화비, 기타교통비 (차량), 기타지출비, 예비비충당 (토스뱅크), 기타), 항목, 항목상세, 
	// 결제수단 (토스뱅크카드, 케이뱅크카드, 부천페이, 카카오뱅크체크, 하나카드, 신한딥드림, 신한카카오페이, 부산썸뱅크, 국민체크7157, 현금결제)
	// 결제금액, 비고

	let data_date;
	let data_category;
	let data_item;
	let data_item_detail;
	let data_pay_method;
	let data_price;
	let data_etc;

	const handleSubmit = () => {
		console.log('submitted');
	}
</script>

<main>
	<div id="container">
		<h1>내 <span class="title-impact">머니머니</span> 등록</h1>
		<form on:submit|preventDefault={handleSubmit}>
			<input type="text">
			<button type="submit">적용</button>
		</form>
	</div>
</main>

<style>
	:global(body) {
		background-color: #0f2020 !important;
		color: #bfc2c7;
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
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>