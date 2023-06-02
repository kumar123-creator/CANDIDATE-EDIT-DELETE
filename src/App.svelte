<script>
	import { onMount } from "svelte";
	import { createEventDispatcher } from "svelte";
	import 'bootstrap/dist/css/bootstrap.min.css';
  import EditJobPopUp from "../EditJobPopUp.svelte";
  import DeletePopup from "../DeletePopup.svelte";
  import UploadCvPopup from "../UploadCvPopup.svelte";
  
	let jsonData = [];
	let tableVisible = false;
	let selectedJob = null;
	let uploadJobId = null;
	let deleteJobId = null;
  let editJob = null;
  
	const dispatch = createEventDispatcher();
  
	onMount(async () => {
	  await fetchData();
	  tableVisible = true;
	});
  
	async function fetchData() {
	  const response = await fetch("https://api.recruitly.io/api/candidate?apiKey=TEST1236C4CF23E6921C41429A6E1D546AC9535E");
	  const responseData = await response.json();
	  jsonData = responseData.data;
	}
  
	function handleTitleClick(job) {
	  selectedJob = job;
	  dispatch("showPopup");
	}
  
	function handleUploadCV(jobId) {
	  uploadJobId = jobId;
	  isPopupVisible = true;
	}
  
	function handleFileUpload(event) {
	  file = event.target.files[0];
	  // Perform further actions with the uploaded file
  
	  // Example: Update the backend API URL with the file upload
	  const formData = new FormData();
	  formData.append("file", file);
  
	  fetch(`https://api.recruitly.io/api/candidatecv/upload?apiKey=TEST1236C4CF23E6921C41429A6E1D546AC9535E&candidateId=${uploadJobId}`, {
		method: "POST",
		body: formData
	  })
		.then(response => {
		  // Handle the response accordingly
		  if (response.ok) {
			console.log("CV uploaded successfully!");
			isUploadSuccess = true; // Set the flag to true
		  } else {
			console.error("CV upload failed.");
		  }
		})
		.catch(error => {
		  console.error("CV upload error:", error);
		})
		.finally(() => {
		  // Perform close logic
		  isPopupVisible = false;
		});
	}
  
  function handleClose() {
	  // Perform close logic
	  isPopupVisible = false;
  }

	function handleDelete(jobId) {
	  deleteJobId = jobId;
	  isDeletePopupVisible = true;
	}
  
	function handleEdit(job) {
	  editJob = { ...job };
	  isEditPopupVisible = true;
	}
 
  </script>
  
  <main class="container mt-4">
	{#if tableVisible}
	  <table class="table">
		<thead class="thead-light">
		  <tr>
			<th>firstName</th>
			<th>surname</th>
			<th>email</th>
			<th>mobile</th>
      <th>ID</th>
			<th>Actions</th>
		  </tr>
		</thead>
		<tbody>
		  {#each jsonData as job}
			<tr>
			  <td>{job.firstName}</td>
			  <td>{job.surname}</td>
			  <td>{job.email}</td>
			  <td>{job.mobile}</td>
        <td>{job.id}</td>
			  <td>
				<button on:click={() => handleEdit(job)} class="btn btn-info">Edit</button>
				<button on:click={() => handleDelete(job.id)} class="btn btn-danger">Delete</button>
        <button on:click={() => handleUploadCV(job.id)} class="btn btn-primary">Upload CV</button>
			  </td>
			</tr>
		  {/each}
		</tbody>
	  </table>
    {/if}
  </main>
