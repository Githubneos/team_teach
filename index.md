---
layout: post
title: Flocker Social Media Site 
search_exclude: true
description: 
hide: true
menu: nav/home.html
---

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Python Interpreter with Skulpt</title>
    <!-- Load Skulpt (Python interpreter) -->
    <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/skulpt/1.1.1/skulpt.min.js"></script>
    <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/skulpt/1.1.1/skulpt-stdlib.js"></script>
    <!-- Load CodeMirror (for VS Code-like editor) -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.62.0/codemirror.min.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.62.0/theme/dracula.min.css">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.62.0/codemirror.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.62.0/mode/python/python.min.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            padding: 20px;
        }
        #output {
            width: 80%;
            height: 150px;
            margin-top: 10px;
            background-color: #f4f4f4;
            padding: 10px;
            overflow-y: auto;
            border: 1px solid #ccc;
            font-family: "Courier New", monospace;
        }
        #python-input {
            width: 80%;
            height: 300px;
        }
        .CodeMirror {
            border: 1px solid #ccc;
            font-size: 16px;
        }
        #run-btn {
            margin-top: 10px;
        }
    </style>
</head>
<body>
    <h1>Python Interpreter with Syntax Highlighting</h1>
    <!-- CodeMirror editor for Python code input -->
    <textarea id="python-input" placeholder="Write your Python code here..."></textarea><br>
    <button id="run-btn" onclick="runPythonCode()">Run Code</button>
    <div id="output"></div>
    <script type="text/javascript">
        // Initialize CodeMirror for syntax highlighting
        var editor = CodeMirror.fromTextArea(document.getElementById('python-input'), {
            mode: 'python',
            theme: 'dracula',
            lineNumbers: true,
            matchBrackets: true
        });
        // Function to handle output
        function outf(text) {
            console.log(text); // For debugging purposes
            document.getElementById("output").innerHTML += text + "<br/>";
        }
        // Function to execute Python code using Skulpt
        function runPythonCode() {
            var pythonCode = editor.getValue();  // Get code from CodeMirror editor
            document.getElementById("output").innerHTML = '';  // Clear previous output
            Sk.configure({
                output: outf,  // Handle the output
                read: function(x) {
                    if (Sk.builtinFiles === undefined || Sk.builtinFiles["files"][x] === undefined)
                        throw "File not found: '" + x + "'";
                    return Sk.builtinFiles["files"][x];
                }
            });
            Sk.misceval.asyncToPromise(function() {
                return Sk.importMainWithBody("<stdin>", false, pythonCode);
            }).then(function(result) {
                console.log("Execution completed");
            }).catch(function(err) {
                outf("Error: " + err.toString());
            });
        }
    </script>
</body>
</html>

