<script>
	import '../style.css';

	// 전원 상태
	let power = false;
	// 풍속 단계: 0=OFF, 1=약, 2=중, 3=강
	let speed = 1;
	// LED 색상
	let ledColor = '#ff7e6b';
	// 다크모드
	let darkMode = false;
	let showBackground = true;
	// 날개 수
	const bladeCount = 12;

	// 풍속 단계별 텍스트
	const speedLabel = ['OFF', '약', '중', '강'];
	// LED 색상 프리셋 (다양한 파스텔 톤)
	const ledColors = ['#ffb7c5', '#ffd600', '#83e86f', '#87ceeb', '#d4a5f0'];

	function togglePower() {
		power = !power;
		if (!power) speed = 0;
		else if (speed === 0) speed = 1;
	}

	/**
	 * 풍속 단계 변경
	 * @param {number} n
	 */
	function changeSpeed(n /** @type {number} */) {
		if (power) speed = n;
	}

	/**
	 * LED 색상 변경
	 * @param {string} color
	 */
	function changeLedColor(color /** @type {string} */) {
		ledColor = color;
	}

	function toggleDark() {
		darkMode = !darkMode;
		document.documentElement.classList.toggle('dark', darkMode);
	}

	function toggleBackground() {
		showBackground = !showBackground;
		const app = document.querySelector('.fan-app');
		if (app) {
			app.style.setProperty('--background-url', showBackground ? 'url(/background.jpg)' : 'none');
		}
	}
</script>

<main class="fan-app {darkMode ? 'dark' : ''}">
	<!-- 핸디 선풍기 일러스트 -->
	<div class="fan-illustration">
		<div class="fan-body">
			<div class="fan-head" style="box-shadow: 0 0 30px 10px {ledColor};">
				<div class="fan-blades-container">
					<div class="fan-blades {power ? 'spin speed-' + speed : ''}">
						{#each Array(bladeCount) as _, i}
							<div class="blade" style="--i: {i};"></div>
						{/each}
					</div>
				</div>
				<div class="fan-center"></div>
			</div>
			<div class="fan-stick"></div>
			<div class="fan-base"></div>
		</div>
	</div>

	<!-- 컨트롤 패널 -->
	<div class="panel">
		<button class="btn power" on:click={togglePower} aria-label="전원">
			{power ? 'OFF' : 'ON'}
		</button>
		<div class="speed-controls">
			{#each [1, 2, 3] as n}
				<button
					class="btn speed {speed === n ? 'active' : ''}"
					on:click={() => changeSpeed(n)}
					disabled={!power}
				>
					{speedLabel[n]}
				</button>
			{/each}
		</div>
		<div class="led-controls">
			LED
			{#each ledColors as c}
				<button
					class="led-dot"
					style="background:{c}; border: {ledColor === c ? '2px solid #222' : 'none'}"
					on:click={() => changeLedColor(c)}
					aria-label="LED 색상 선택"
				></button>
			{/each}
		</div>
		<button class="btn dark-toggle" on:click={toggleDark}>
			{darkMode ? '☀️' : '🌙'}
		</button>
		<button class="btn bg-toggle" on:click={toggleBackground}>
			{showBackground ? '바다 숨기기' : '바다 보기'}
		</button>
	</div>
</main>

<!--
  🧊 super-cool-fan
  - 핸디 선풍기 일러스트와 컨트롤 패널 UI
  - 전원, 풍속, LED, 다크모드 지원
  - 디자인: 시원하고 현대적인 느낌
  - 작성자: hyeyseo
-->
