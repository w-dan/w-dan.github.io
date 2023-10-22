<script>
    import { onMount } from 'svelte';

    let leftText = "";
    let rightText = "";
    let index = 0;
    let score = 0;
    let isMalformed = false;
    let responseMessage = "";
    const url = "https://script.google.com/macros/s/AKfycbwrA9EiUAehpy3NloMFP_wfvXTzFFMJiEMegNRf0XEV3FF7rnmfZ7Zn_54ukp7fyFa7/exec";

    const fetchRandomRow = () => {
        // Implement Google Sheets API request to fetch a random row
        // and set the values of leftText and rightText.
        // This requires setting up and authenticating the Google Sheets API.
        // Refer to Google Sheets API documentation for details.

        fetch(`${url}?action=getRandomData`)
        .then(response => response.json())
        .then(data => {
            console.log(data);
            leftText = data["A"];
            rightText = data["B"];
            index = data["index"];
            // Use the data from 'data.A' and 'data.B' as needed.
        })
        .catch(error => {
            console.log('Error:', error);
            alert("Some error occurred, please contact us. Sorry.");
        });
    };

    onMount(() => {
        fetchRandomRow(); // Fetch the initial random row on page load.
    });

    const handleSubmit = async () => {
        console.log(index);
        const data = {
            score,
            isMalformed,
            index,
        };

        fetch(`${url}?action=saveData&score=${score}&isMalformed=${isMalformed}&index=${index}`)
        .then(response => { console.log(response); return response.json(); })
        .then(data => {
            responseMessage = data.message;
            // Use the data from 'data.A' and 'data.B' as needed.
        })
        .catch(error => {
            console.log('Error:', error);
            alert("Some error occurred, please contact us. Sorry.");
        });
    };

    const getNewAnswer = () => {
        leftText = "";
        rightText = "";
        score = 0;
        isMalformed = false;
        responseMessage = "";
        fetchRandomRow();
    };
</script>

<main>
<h1>Random Row Data</h1>
<div>
    <div class="left-column">
    <p>{leftText}</p>
    </div>
    <div class="right-column">
    <p>{rightText}</p>
    </div>
</div>

<form on:submit={handleSubmit}>
    <label for="score">Score:</label>
    <input type="number" bind:value={score} min="0" max="10" required />

    <label for="malformed">Malformed:</label>
    <input type="checkbox" bind:checked={isMalformed} />

    <button type="submit">Submit</button>
</form>

{#if responseMessage}
  <p>{responseMessage}</p>
{/if}

<button on:click={getNewAnswer}>Get New Answer</button>
</main>

<style>

    main {
        margin: auto;
        width: 80vw;
    }

    h1 {
      font-size: 24px;
      margin-bottom: 20px;
    }
  
    div {
      display: flex;
      justify-content: space-between;
      margin-bottom: 20px;
    }
  
    .left-column, .right-column {
      flex: 1;
      padding: 10px;
      border: 1px solid #ccc;
      background-color: #f5f5f5;
    }
  
    form {
    width: 300px;
    margin: 0 auto;
    padding: 20px;
    background-color: #f4f4f4;
    border: 1px solid #ccc;
    border-radius: 5px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    }

    label {
    display: block;
    margin-bottom: 5px;
    font-weight: bold;
    }

    input[type="number"],
    input[type="checkbox"] {
    width: 100%;
    padding: 10px;
    margin-bottom: 10px;
    border: 1px solid #ccc;
    border-radius: 3px;
    }

    button[type="submit"] {
    background-color: #007bff;
    color: #fff;
    border: none;
    padding: 10px 20px;
    border-radius: 5px;
    cursor: pointer;
    }

    button[type="submit"]:hover {
    background-color: #0056b3;
    }

    /* Adjust the styling of the checkbox label */
    input[type="checkbox"] + label {
    display: inline-block;
    vertical-align: middle;
    margin-left: 5px;
    }

</style>
  
