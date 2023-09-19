<!-- UploadButton.svelte -->
<script>
	let inputRef;
	let isModalOpen = false;

	function handleFileInputChange() {
		inputRef.click(); // Trigger the file input click event when the button is clicked
	}

	function handleFileInput(event) {
		const selectedFile = event.target.files[0];
		if (selectedFile) {
			// Handle the selected file here (e.g., you can upload it or process it)
			// You can emit an event to notify the parent component about the selected file

			// For question answering, you can define a function to handle it
			performQuestionAnswering(selectedFile);
		}
	}

	async function performQuestionAnswering(document) {
		try {
			// Initialize the question answering pipeline
			const { DocumentQuestionAnsweringPipeline } = require("@xenova/transformers");
			const documentQA = new DocumentQuestionAnsweringPipeline();

			// Example question
			const question = "What is the main topic of this document?";

			// Perform question answering
			const answers = await documentQA(document, question);

			// Display the answers to the user or handle them as needed
			console.log("Answers:", answers);
		} catch (error) {
			console.error("Question answering error:", error);
		}
	}

	// Function to close the modal
	function closeModal() {
		isModalOpen = false;
	}

	// Toggle the modal when clicking the button
	function toggleModal() {
		isModalOpen = !isModalOpen;
	}
</script>

<div class="container">
	<div>
		<input
			type="file"
			accept=".pdf, .doc, .docx"
			bind:this={inputRef}
			on:change={handleFileInput}
			style="display: none;"
		/>
		<button class="btn-upload-document" on:click={handleFileInputChange}>+</button>
	</div>

	<div class="modal" style="display: {isModalOpen ? 'block' : 'none'}">
		<div class="modal-content">
			<!-- Make "Upload File" text clickable -->
			<h2 style="cursor: pointer; margin-bottom: 10px;" on:click={handleFileUpload}>Upload File</h2>
			<p>PDF, DOC, DOCX</p>
			<!-- Remove the duplicate "Upload File" button -->
		</div>
	</div>

	<div class="chatbox-container">
		<!-- Your chatbox component code goes here -->
	</div>
</div>

<style>
	/* Add your custom styles for the button as needed */
	.btn-upload-document {
		cursor: pointer;
		background-color: #3490dc; /* Background color */
		color: #ffffff; /* Text color */
		padding: 8px 16px;
		border: none;
		border-radius: 50%; /* Rounded shape for the button */
		font-size: 24px; /* Adjust font size as needed */
	}

	.btn-upload-document:hover {
		background-color: #2779bd; /* Hover background color */
	}

	.chatbox-container {
		/* Add styles for your chatbox container here */
	}

	.container {
		display: flex;
	}

	/* Smaller Modal Styles */
	.modal {
		display: none;
		position: absolute;
		top: -80px;
		left: -10%;
		transform: translateX(-50%);
		width: 200px;
		background-color: #fff;
		border: 1px solid #ccc;
		box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
		border-radius: 10px; /* Make it rounder */
		z-index: 1;
	}

	.modal-content {
		padding: 10px;
		text-align: center;
		color: #999; /* Lighter font color */
		font-weight: lighter; /* Lighter font weight */
	}
</style>
