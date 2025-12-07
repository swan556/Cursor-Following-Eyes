<script lang="ts">
	let pointer_coor: { x: number; y: number } = $state({ x: 0, y: 0 });
	const onpointermove = (event: any) => {
		pointer_coor.x = event.clientX;
		pointer_coor.y = event.clientY;
	};
</script>

<div class="container" {onpointermove}>
	<div class="eyes">
		<div class="eye">
			<div class="pupil left">
				<p>{Math.round(pointer_coor.x)}</p>
			</div>
		</div>
		<div class="eye">
			<div class="pupil right">
				<p>{Math.round(pointer_coor.y)}</p>
			</div>
		</div>
	</div>
</div>

<style>
	:global(body) {
		background-color: rgb(42, 42, 42);
		padding: 0;
		margin: 0;
		height: 100%;
		width: 100%;
	}

	.container {
		align-items: center;
		display: flex;
		justify-content: center;
		height: 100dvh;
		width: 100dvw;
	}

	.eyes {
		align-items: center;
		display: flex;
		flex-direction: row;
		justify-content: center;
	}
	.eye {
		background-color: azure;
		width: 100px;
		height: 100px;
		border: 0px solid;
		border-radius: 50%;
		margin: 10px;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.pupil {
		background-color: rgb(0, 169, 169);
		width: 20px;
		height: 40px;
		border: 0px solid;
		border-radius: 50%;
	}
</style>
