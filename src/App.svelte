<script>
	let feedback = '';
	let statusMessage = '';
  
	const submitFeedback = async () => {
	  const response = await fetch('https://feedback-worker.samanuesam.workers.dev', {
		method: 'POST',
		headers: { 'Content-Type': 'application/json' },
		body: JSON.stringify({ feedback })
	  });
  
	  const result = await response.json();
	  statusMessage = result.message;
	  feedback = '';
	};
  </script>
  
  <main>
	<h1>User Feedback</h1>
	<form on:submit|preventDefault={submitFeedback}>
	  <textarea bind:value={feedback} placeholder="Enter your feedback"></textarea>
	  <button type="submit">Submit</button>
	</form>
	{#if statusMessage}
	  <p>{statusMessage}</p>
	{/if}
  </main>
  
  <style>
	main { text-align: center; padding: 2rem; }
	textarea { width: 80%; height: 100px; margin-bottom: 1rem; }
	button { padding: 0.5rem 1rem; }
	p { color: green; font-weight: bold; }
  </style>
  