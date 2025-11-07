# Frontend_project
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Secure Cloud Storage</title>

    <style>
      body {
        font-family: Arial, sans-serif;
        background: #f8f9fa;
        margin: 0;
        padding: 0;
      }
      header {
        background: #343a40;
        padding: 15px;
        color: white;
        text-align: center;
        font-size: 22px;
        font-weight: bold;
      }
      .container {
        width: 500px;
        background: white;
        margin: 30px auto;
        padding: 25px;
        border-radius: 8px;
        box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.15);
      }
      input[type="file"] {
        width: 100%;
        padding: 10px;
        margin-bottom: 10px;
      }
      button {
        background: #007bff;
        padding: 10px 20px;
        border: none;
        color: white;
        cursor: pointer;
        border-radius: 5px;
      }
      button:hover {
        opacity: 0.85;
      }
      .fileList {
        margin-top: 20px;
      }
      table {
        width: 100%;
        border-collapse: collapse;
      }
      td,
      th {
        border: 1px solid #ddd;
        padding: 8px;
        text-align: center;
      }
    </style>
  </head>
  <body>
    <header>Secure Cloud File Storage System</header>

    <div class="container">
      <h3>Upload File</h3>
      <input type="file" id="fileInput" />
      <button onclick="uploadFile()">Upload</button>

      <br /><br />
      <hr />
      <br />

      <h3>Available Files</h3>
      <div class="fileList">
        <table id="filesTable">
          <tr>
            <th>File Name</th>
            <th>Action</th>
          </tr>
        </table>
      </div>
    </div>

    <script>
      // Load available files on page load
      window.onload = loadFiles;

      async function uploadFile() {
        const file = document.getElementById("fileInput").files[0];
        if (!file) {
          alert("Select a file first!");
          return;
        }

        let formData = new FormData();
        formData.append("file", file);

        let res = await fetch("/upload", { method: "POST", body: formData });
        if (res.ok) {
          alert("File Uploaded Successfully!");
          loadFiles();
        } else {
          alert("Upload Failed!");
        }
      }

      async function loadFiles() {
        let res = await fetch("/files");
        let data = await res.json();

        let table = document.getElementById("filesTable");
        table.innerHTML = "<tr><th>File Name</th><th>Action</th></tr>";

        data.forEach((f) => {
          table.innerHTML += `
                <tr>
                    <td>${f.filename}</td>
                    <td><button onclick="downloadFile('${f.file_id}')">Download</button></td>
                </tr>
            `;
        });
      }

      function downloadFile(file_id) {
        window.location.href = "/download/" + file_id;
      }
    </script>
  </body>
</html>
