<script>
	let feedback = '';
	let statusMessage = '';
	let feedbackList = [];

	// Fetch all feedback from the server
	const fetchFeedback = async () => {
		try {
			const response = await fetch('https://feedback-worker.samanuesam.workers.dev');
			if (!response.ok) throw new Error('Failed to fetch feedback');
			feedbackList = await response.json();
		} catch (error) {
			console.error(error);
			statusMessage = 'Error fetching feedback!';
		}
	};

	// Submit feedback and refresh the list
	const submitFeedback = async () => {
		try {
			const response = await fetch('https://feedback-worker.samanuesam.workers.dev', {
				method: 'POST',
				headers: { 'Content-Type': 'application/json' },
				body: JSON.stringify({ feedback })
			});
			const result = await response.json();
			statusMessage = result.message;
			feedback = '';

			// Refresh feedback list
			await fetchFeedback();
		} catch (error) {
			console.error(error);
			statusMessage = 'Error submitting feedback!';
		}
	};

	// Fetch feedback when component loads
	fetchFeedback();
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

	<h2>Previous Feedback</h2>
	{#if feedbackList.length > 0}
		<ul>
			{#each feedbackList as item}
				<li>{item.message} <small>({item.timestamp})</small></li>
			{/each}
		</ul>
	{:else}
		<p>No feedback available yet.</p>
	{/if}
</main>

<style>
	main {
		text-align: center;
		padding: 2rem;
	}
	textarea {
		width: 80%;
		height: 100px;
		margin-bottom: 1rem;
	}
	button {
		padding: 0.5rem 1rem;
	}
	p {
		color: green;
		font-weight: bold;
	}
	h2 {
		margin-top: 2rem;
	}
	ul {
		list-style-type: none;
		padding: 0;
	}
	li {
		background: #f3f3f3;
		margin: 0.5rem auto;
		padding: 0.5rem;
		width: 80%;
		border-radius: 5px;
	}
</style>
