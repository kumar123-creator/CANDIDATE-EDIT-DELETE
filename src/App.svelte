<script>
  import { onMount } from "svelte";
  import { createEventDispatcher } from "svelte";
  import 'bootstrap/dist/css/bootstrap.min.css';

  let jsonData = [];
  let tableVisible = false;
  let selectedCandidate = null;
  let editedCandidate = null;
  let showSuccessMessage = false;
  let showDeleteConfirmation = false;

  const dispatch = createEventDispatcher();

  onMount(async () => {
    await fetchData();
    tableVisible = true;
  });

  async function fetchData() {
    const response = await fetch("https://api.recruitly.io/api/candidate/?apiKey=TEST1236C4CF23E6921C41429A6E1D546AC9535E");
    const responseData = await response.json();
    jsonData = responseData.data;
  }

  function handleTitleClick(candidate) {
    selectedCandidate = candidate;
    editedCandidate = { ...selectedCandidate };
  }

  async function saveCandidate() {
    Object.assign(selectedCandidate, editedCandidate);

    // Find the index of the selectedCandidate in the jsonData array
    const index = jsonData.findIndex(candidate => candidate.id === selectedCandidate.id);

    // Update the corresponding item in the jsonData array
    jsonData[index] = { ...selectedCandidate };

    // Send the updated candidate data to the API

    showSuccessMessage = true;

    closePopup();
  }

  function deleteCandidate() {
    // Find the index of the selectedCandidate in the jsonData array
    const index = jsonData.findIndex(candidate => candidate.id === selectedCandidate.id);

    // Remove the selectedCandidate from the jsonData array
    jsonData.splice(index, 1);

    // Send the delete request to the API

    showSuccessMessage = true;

    closePopup();
  }

  function closePopup() {
    selectedCandidate = null;
    editedCandidate = null;
    showDeleteConfirmation = false;
  }
</script>

<style>
  .popup {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .popup-content {
    background-color: #ffffff;
    padding: 40px;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
    max-width: 400px;
    width: 90%;
    text-align: center;
  }

  .popup-content h1 {
    color: blue;
    font-size: 24px;
    margin-bottom: 25px;
  }

  .popup-content p {
    margin-bottom: 10px;
  }

  .popup-content input {
    width: 100%;
    padding: 7px;
    margin-bottom: 15px;
  }

  .popup-content button {
    margin-top: 20px;
  }

  .success-popup {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .success-popup-content {
    background-color: #ffffff;
    padding: 40px;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
    max-width: 400px;
    width: 90%;
    text-align: center;
  }
</style>

<main class="container mt-4">
  {#if tableVisible}
    <div class="table-container">
      <table class="table">
        <thead class="thead-light">
          <tr>
            <th>First Name</th>
            <th>Surname</th>
            <th>Email</th>
            <th>Mobile</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          {#each jsonData as candidate}
            <tr>
              <td>
                <a href="#" on:click|preventDefault={() => handleTitleClick(candidate)}>{candidate.firstName}</a>
              </td>
              <td>{candidate.surname}</td>
              <td>{candidate.email}</td>
              <td>{candidate.mobile}</td>
              <td>
                <button class="btn btn-primary" on:click|preventDefault={() => handleTitleClick(candidate)}>Edit</button>
                <button class="btn btn-danger" on:click|preventDefault={() => showDeleteConfirmation = true}>Delete</button>
              </td>
            </tr>
          {/each}
        </tbody>
      </table>
    </div>
  {/if}
</main>

{#if selectedCandidate}
<div class="popup">
  <div class="popup-content">
    <h1 style="color:blue;">Candidate Details</h1>
    <p>First Name: <input type="text" bind:value={editedCandidate.firstName} placeholder="Enter first name" /></p>
    <p>Surname: <input type="text" bind:value={editedCandidate.surname} placeholder="Enter surname" /></p>
    <p>Email: <input type="text" bind:value={editedCandidate.email} placeholder="Enter email" /></p>
    <p>Mobile: <input type="text" bind:value={editedCandidate.mobile} placeholder="Enter mobile" /></p>
    <p>CV Upload: <input type="file" /></p>
    <div>
      <button class="btn btn-primary" on:click={saveCandidate}>Save</button>
      <button class="btn btn-secondary" on:click={closePopup}>Close</button>
    </div>
  </div>
</div>
{/if}

{#if showDeleteConfirmation}
<div class="popup">
  <div class="popup-content">
    <h1 style="color:red;">Delete Confirmation</h1>
    <p>Are you sure you want to delete this candidate?</p>
    <div>
      <button class="btn btn-danger" on:click={deleteCandidate}>Delete</button>
      <button class="btn btn-secondary" on:click={closePopup}>Cancel</button>
    </div>
  </div>
</div>
{/if}

{#if showSuccessMessage}
<div class="success-popup">
  <div class="success-popup-content">
    <h1>Success!</h1>
    <p>Your details have been successfully updated.</p>
    <button class="btn btn-primary" on:click={() => showSuccessMessage = false}>Close</button>
  </div>
</div>
{/if}
