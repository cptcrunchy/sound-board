<script>
	import { onMount } from 'svelte'

	let media = []
	let mediaRecorder = null;
	onMount(async () => {
		const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
		
		mediaRecorder = new MediaRecorder(stream);
		mediaRecorder.ondataavailable = (e) => media.push(e.data)
		mediaRecorder.onstop = function(){
			const audio = document.querySelector('audio');
			const blob = new Blob( media, {'type' : 'audio/ogg; codecs=opus' });
			media = [];
			audio.src = window.URL.createObjectURL(blob);
		}
		
	})
	
	function startRecording(){
		mediaRecorder.start();
	}

	function stopRecording(){
		mediaRecorder.stop();
	}

</script>

<section>
	<h2>Sound Board</h2>
	<audio controls/>
	<button on:click={startRecording}>Record</button>
	<button on:click={stopRecording}>Stop</button>
</section>

<style>
	section{
		display: flex;
		flex-flow: column;
		width: 300px;
	}
</style>