<script>
    import { createEventDispatcher } from "svelte";
    
    export let jobId;
    
    let isPopupVisible = true;
    let file = null;
    let isUploadSuccess = false;
    
    const dispatch = createEventDispatcher();
    
    function handleFileUpload(event) {
      file = event.target.files[0];
      // Perform further actions with the uploaded file
    }
    
    function handleSave() {
      const formData = new FormData();
      formData.append("file", file);
    
      fetch(`https://api.recruitly.io/api/candidatecv/upload?apiKey=TEST1236C4CF23E6921C41429A6E1D546AC9535E&candidateId=${jobId}`, {
        method: "POST",
        body: formData
      })
        .then(response => {
          if (response.ok) {
            console.log("CV uploaded successfully!");
            isUploadSuccess = true;
          } else {
            console.error("CV upload failed.");
          }
        })
        .catch(error => {
          console.error("CV upload error:", error);
        })
        .finally(() => {
          isPopupVisible = false;
        });
    }
    
    function handleClose() {
      isPopupVisible = false;
    }
  </script>
  
  <div class="popup">
    <h4>Upload CV</h4>
    <input type="file" on:change={handleFileUpload} />
    <button on:click={handleSave} class="btn btn-primary">Save</button>
    <button on:click={handleClose} class="btn btn-secondary">Close</button>
  </div>
  
  {#if isUploadSuccess}
    <div class="popup success">
      <h4>CV Uploaded Successfully!</h4>
      <button on:click={() => isUploadSuccess = false} class="btn btn-primary">OK</button>
    </div>
  {/if}
  
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
        background-color: lightgreen;
    }
  </style>
  