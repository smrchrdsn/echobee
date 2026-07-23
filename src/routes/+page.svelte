<script lang="ts">
	let mediaRecorder: MediaRecorder | null = null;
	let audioSrc = '';
	let audioEl: HTMLAudioElement | undefined;
	let isRecording = $state(false);

	function startRecording(event?: PointerEvent) {
		if (event) {
			(event.target as HTMLButtonElement).setPointerCapture(event.pointerId);
		}
		isRecording = true;
		navigator.mediaDevices.getUserMedia({ audio: true }).then((stream) => {
			mediaRecorder = new MediaRecorder(stream);
			mediaRecorder.start();

			const audioChunks: BlobPart[] = [];
			mediaRecorder.addEventListener('dataavailable', (event) => {
				audioChunks.push(event.data);
			});

			mediaRecorder.addEventListener('stop', () => {
				const blob = new Blob(audioChunks, { type: 'audio/mp4' });
				audioSrc = URL.createObjectURL(blob);
				if (audioEl) {
					audioEl.src = audioSrc;
					audioEl.play();
				}
			});
		});
	}

	function stopRecording() {
		mediaRecorder?.stop();
		mediaRecorder = null;
		isRecording = false;
	}

	function handleKey(event: KeyboardEvent) {
		if (event.key === ' ') {
			event.preventDefault();
			if (event.type === 'keydown' && !event.repeat) {
				startRecording();
			} else if (event.type === 'keyup' && !event.repeat) {
				stopRecording();
			}
		}
	}
</script>

<svelte:window on:keydown={handleKey} on:keyup={handleKey} />

<h1>echobee</h1>
<button
	aria-label="Record"
	class={isRecording ? 'active' : ''}
	onpointerdown={startRecording}
	onpointerup={stopRecording}
></button>
<audio bind:this={audioEl}></audio>

<style lang="scss">
	button {
		background-color: var(--red-9);
		block-size: 64px;
		aspect-ratio: 1 / 1;
		border: 4px solid var(--red-7);
		border-radius: 50%;

		&:is(:active, .active) {
			background-color: var(--red-11);
			border-color: var(--red-9);
		}
	}
</style>
