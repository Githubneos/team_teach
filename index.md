---
layout: post
title: Flocker Social Media Site 
search_exclude: true
description: 
hide: true
menu: nav/home.html
---

[2025-03-20 Team Teach Notebook](/path/to/notebook/2025-03-20-team-teach.ipynb)


<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Replit-like Python Interpreter</title>
  <!-- CodeMirror CSS -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/codemirror.min.css">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/theme/dracula.min.css">
  <style>
    body {
      margin: 0;
      padding: 0;
      background-color: #282a36;
      color: #f8f8f2;
      font-family: sans-serif;
      overflow: auto; /* Allow full page scrolling */
    }
    .container {
      display: flex;
      flex-wrap: wrap;
      min-height: 100vh;
      width: 100%;
      padding: 10px;
      box-sizing: border-box;
    }
    .editor-section, .output-section {
      flex: 1;
      min-width: 300px;
      display: flex;
      flex-direction: column;
      padding: 10px;
      box-sizing: border-box;
    }
    .editor-section {
      border-right: 1px solid #44475a;
    }
    .header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 10px;
    }
    #run-btn {
      background-color: #50fa7b;
      border: none;
      border-radius: 3px;
      padding: 5px 10px;
      color: #282a36;
      font-weight: bold;
      cursor: pointer;
    }
    /* CodeMirror editor styling */
    .CodeMirror {
      flex: 1;
      border: 1px solid #44475a;
      overflow: auto;
      height: auto;
    }
    /* Output container styling */
    #output {
      flex: 1;
      background-color: #1e1f29;
      border: 1px solid #44475a;
      padding: 10px;
      overflow-y: auto;
      white-space: pre-wrap;
      font-family: monospace;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="editor-section">
      <div class="header">
        <h2 style="margin: 0; font-size: 18px;">Python Editor</h2>
        <!-- Inline onclick for immediate response -->
        <button id="run-btn" onclick="runPython()">Run</button>
      </div>
      <textarea id="code">
print("Hello, Replit-like world!")
for i in range(50):
    print("Line", i+1)
      </textarea>
    </div>
    <div class="output-section">
      <div class="header">
        <h2 style="margin: 0; font-size: 18px;">Output</h2>
      </div>
      <div id="output"></div>
    </div>
  </div>
  
  <!-- CodeMirror JS -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/codemirror.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/mode/python/python.min.js"></script>
  <!-- Skulpt for Python execution using updated CDN links -->
  <script src="https://cdn.jsdelivr.net/npm/skulpt@1.2.0/dist/skulpt.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/skulpt@1.2.0/dist/skulpt-stdlib.js"></script>
  
  <script>
    var editor;
    window.onload = function() {
      // Initialize CodeMirror on the textarea after the page loads
      editor = CodeMirror.fromTextArea(document.getElementById("code"), {
        mode: "python",
        theme: "dracula",
        lineNumbers: true,
        indentUnit: 4,
        tabSize: 4,
        viewportMargin: Infinity,
      });
    };

    // Skulpt file loader
    function builtinRead(x) {
      if (Sk.builtinFiles === undefined || Sk.builtinFiles["files"][x] === undefined) {
        throw "File not found: '" + x + "'";
      }
      return Sk.builtinFiles["files"][x];
    }

    // Function to run the Python code
    function runPython() {
      console.log("Run button clicked");
      var outputElement = document.getElementById("output");
      outputElement.innerHTML = "";
      Sk.configure({
        output: function(text) {
          outputElement.innerHTML += text;
        },
        read: builtinRead,
      });
      var prog = editor.getValue();
      Sk.misceval.asyncToPromise(function() {
        return Sk.importMainWithBody("<stdin>", false, prog, true);
      }).catch(function(err) {
        outputElement.innerHTML = err.toString();
      });
    }
  </script>
</body>
</html>

