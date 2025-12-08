<script lang="ts">
	const max_x: number = 1407;
	const max_y: number = 712;

	const origin_true: { x: number; y: number } = { x: max_x / 2, y: max_y / 2 };
	const eye_left_origin: { x: number; y: number } = { x: 60, y: 0 };
	const eye_right_origin: { x: number; y: number } = { x: -60, y: 0 };
	let pointer_coor: { x: number; y: number } = $state({ x: 0, y: 0 });
	let dir_from_left_eye: { x: number; y: number } = $derived({
		x: pointer_coor.x - eye_left_origin.x,
		y: pointer_coor.y - eye_left_origin.y
	});
	let dir_from_right_eye: { x: number; y: number } = $derived({
		x: pointer_coor.x - eye_right_origin.x,
		y: pointer_coor.y - eye_right_origin.y
	});

	let dist_from_left_eye: number = $derived(
		Math.sqrt(Math.pow(dir_from_left_eye.x, 2) + Math.pow(dir_from_left_eye.y, 2))
	);
	let dist_from_right_eye: number = $derived(
		Math.sqrt(Math.pow(dir_from_right_eye.x, 2) + Math.pow(dir_from_right_eye.y, 2))
	);

	let left_eye_dir_theta: number = $derived(Math.atan(dir_from_left_eye.y / dir_from_left_eye.x));
	let right_eye_dir_theta: number = $derived(
		Math.atan(dir_from_right_eye.y / dir_from_right_eye.x)
	);

	let translate_dist_left: { dx_t: number; dy_t: number } = $derived({
		dx_t:
			Math.min(dist_from_left_eye, 30) *
			Math.cos(left_eye_dir_theta) *
			(dir_from_left_eye.x > 0 ? -1 : 1),
		dy_t:
			Math.min(dist_from_left_eye, 30) *
			Math.sin(left_eye_dir_theta) *
			(dir_from_left_eye.x > 0 ? -1 : 1)
	});
	let translate_dist_right: { dx_t: number; dy_t: number } = $derived({
		dx_t:
			Math.min(dist_from_right_eye, 30) *
			Math.cos(right_eye_dir_theta) *
			(dir_from_right_eye.x > 0 ? -1 : 1),
		dy_t:
			Math.min(dist_from_right_eye, 30) *
			Math.sin(right_eye_dir_theta) *
			(dir_from_right_eye.x > 0 ? -1 : 1)
	});

	const onpointermove = (event: any) => {
		pointer_coor.x = -event.clientX + origin_true.x;
		pointer_coor.y = -event.clientY + origin_true.y;
	};
</script>

<div class="container" {onpointermove}>
	<div class="eyes">
		<div class="eye">
			<div
				class="pupil"
				style={`transform: translate(${translate_dist_left.dx_t}px, ${translate_dist_left.dy_t}px);`}
			>
				<!-- <p>{Math.round(pointer_coor.x)}</p> -->
			</div>
		</div>
		<div class="eye">
			<div
				class="pupil"
				style={`transform: translate(${translate_dist_right.dx_t}px, ${translate_dist_right.dy_t}px);`}
			>
				<!-- <p>{Math.round(pointer_coor.y)}</p> -->
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
		background-color: rgb(238, 16, 16);
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
		background-color: rgb(0, 0, 0);
		width: 30px;
		height: 30px;
		border: 3px solid rgb(7, 103, 147);
		border-radius: 50%;
		box-sizing: border-box;
	}
</style>
