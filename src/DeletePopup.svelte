<script>
    import { createEventDispatcher } from "svelte";
    
    export let jobId;
    
    let isDeletePopupVisible = true;
    
    const dispatch = createEventDispatcher();
    
    function handleDeleteConfirm() {
      fetch(`https://api.recruitly.io/api/candidate/${jobId}?apiKey=TEST1236C4CF23E6921C41429A6E1D546AC9535E`, {
        method: "DELETE"
      })
        .then(response => {
          if (response.ok) {
            console.log("Job deleted successfully!");
            // Dispatch event or handle the functionality here
          } else {
            console.error("Job delete failed.");
          }
        })
        .catch(error => {
          console.error("Job delete error:", error);
        })
        .finally(() => {
          isDeletePopupVisible = false;
        });
    }
    
    function handleClose() {
      isDeletePopupVisible = false;
    }
  </script>
  
  <div class="popup">
    <h4>Confirm Delete</h4>
    <p>Are you sure you want to delete this job?</p>
    <button on:click={handleDeleteConfirm} class="btn btn-danger">Delete</button>
    <button on:click={handleClose} class="btn btn-secondary">Cancel</button>
  </div>
 
 <style>
	.popup {
	  position: fixed;
	  top: 50%;
	  left: 50%;
	  transform: translate(-50%, -50%);
	  background-color: white;
	  padding: 20px;
	  border-radius: 8px;
	  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
	}
  
	.success {
	  background-color: blueviolet;
	}
  </style>
  