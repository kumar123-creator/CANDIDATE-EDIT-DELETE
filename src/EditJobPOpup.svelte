<script>
    import { createEventDispatcher } from "svelte";
    
    export let job;
    
    let isEditPopupVisible = true;
    let editedJob = { ...job };
    
    const dispatch = createEventDispatcher();
    
    function handleEditSave() {
      fetch(`https://api.recruitly.io/api/candidate?apiKey=TEST1236C4CF23E6921C41429A6E1D546AC9535E`, {
        method: "POST",
        body: JSON.stringify(editedJob),
        headers: {
          "Content-Type": "application/json"
        }
      })
        .then(response => {
          if (response.ok) {
            console.log("Job edited successfully!");
            // Dispatch event or handle the functionality here
          } else {
            console.error("Job edit failed.");
          }
        })
        .catch(error => {
          console.error("Job edit error:", error);
        })
        .finally(() => {
          isEditPopupVisible = false;
        });
    }
    
    function handleClose() {
      isEditPopupVisible = false;
    }
  </script>
  
  <div class="popup">
    <h4>Edit Job</h4>
    <label for="firstName">First Name:</label>
    <input type="text" id="firstName" bind:value={editedJob.firstName} class="form-control" />
    <label for="surname">Surname:</label>
    <input type="text" id="surname" bind:value={editedJob.surname} class="form-control" />
    <label for="email">Email:</label>
    <input type="email" id="email" bind:value={editedJob.email} class="form-control" />
    <label for="mobile">Mobile:</label>
    <input type="text" id="mobile" bind:value={editedJob.mobile} class="form-control" />
    <button on:click={handleEditSave} class="btn btn-primary">Save</button>
    <button on:click={handleClose} class="btn btn-secondary">Cancel</button>
  </div>
  