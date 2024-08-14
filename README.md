# PDFBrowser
PDF interpreter or ready ?? 

```JS
  // Check if URL is PDF
     function isPdfUrl(url) {
            try {
                // Create a synchronous XMLHttpRequest
                const xhr = new XMLHttpRequest();
                xhr.open('HEAD', url, false); // 'false' makes the request synchronous
                xhr.send();
                
                // Check the Content-Type header
                const contentType = xhr.getResponseHeader('Content-Type');
                if (contentType && contentType.includes('application/pdf')) {
                    return true;
                }

                // If Content-Type is not decisive, fetch the content to check PDF signature
                const xhrGet = new XMLHttpRequest();
                xhrGet.open('GET', url, false); // 'false' makes the request synchronous
                xhrGet.responseType = 'arraybuffer';
                xhrGet.send();

                // Check the first few bytes of the file for PDF signature
                const arrayBuffer = xhrGet.response;
                const uint8Array = new Uint8Array(arrayBuffer);
                const signature = uint8Array.slice(0, 5);
                const pdfSignature = new TextDecoder().decode(signature);

                return pdfSignature === '%PDF-';
            } catch (error) {
                console.error('Error:', error);
                return false;
            }
        }

        // Example usage:
        const url = 'https://myfreightstaff.com/wp-content/uploads/2024/08/Charisma-Ritz-U.pdf';
        const isPdf = isPdfUrl(url);
        console.log('Is the URL a PDF?', isPdf); // true or false

```

PDF.js
PDF.js is an open-source JavaScript library developed by Mozilla that allows you to display PDF files directly in the browser. While it's not a PHP library per se, you can use it in combination with PHP to serve PDF files.

How to use PDF.js:

Include PDF.js in Your Project:
Download PDF.js from the official GitHub repository or use a CDN.

HTML and JavaScript Setup:
Create an HTML file to include the PDF.js viewer.

```HTML
<!DOCTYPE html>
<html>
<head>
    <title>PDF Viewer</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.15.349/pdf_viewer.min.css">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.15.349/pdf.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.15.349/pdf_viewer.min.js"></script>
</head>
<body>
    <div id="pdf-viewer" style="height: 100vh;"></div>
    <script>
        const url = 'path/to/your/file.pdf';
        const pdfjsLib = window['pdfjs-dist/build/pdf'];

        pdfjsLib.GlobalWorkerOptions.workerSrc = 'https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.15.349/pdf.worker.min.js';

        const loadingTask = pdfjsLib.getDocument(url);
        loadingTask.promise.then(function(pdf) {
            console.log('PDF loaded');
            pdf.getPage(1).then(function(page) {
                console.log('Page loaded');
                const scale = 1.5;
                const viewport = page.getViewport({ scale: scale });

                const canvas = document.createElement('canvas');
                const context = canvas.getContext('2d');
                canvas.height = viewport.height;
                canvas.width = viewport.width;
                document.getElementById('pdf-viewer').appendChild(canvas);

                const renderContext = {
                    canvasContext: context,
                    viewport: viewport
                };
                const renderTask = page.render(renderContext);
                renderTask.promise.then(function () {
                    console.log('Page rendered');
                });
            });
        }, function (reason) {
            console.error(reason);
        });
    </script>
</body>
</html>
```

2. PDFObject
PDFObject is a lightweight JavaScript library for embedding PDFs in HTML documents. It’s easy to use and integrates well with PHP for serving PDFs.

How to use PDFObject:

Include PDFObject in Your Project:
Download PDFObject from the official website or use a CDN.

HTML and JavaScript Setup:
Create an HTML file to include the PDFObject viewer.

```HTML

<!DOCTYPE html>
<html>
<head>
    <title>PDF Viewer</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/pdfobject/2.2.8/pdfobject.min.js"></script>
</head>
<body>
    <div id="pdf-viewer" style="height: 100vh;"></div>
    <script>
        PDFObject.embed("path/to/your/file.pdf", "#pdf-viewer");
    </script>
</body>
</html>

```

3. ViewerJS
ViewerJS is another open-source tool that can be used for viewing PDF files and other document types. It’s based on PDF.js and WebODF.

How to use ViewerJS:

Include ViewerJS in Your Project:
Download ViewerJS from the official website or host it locally.

HTML Setup:

```HTML

<!DOCTYPE html>
<html>
<head>
    <title>PDF Viewer</title>
    <link rel="stylesheet" href="path/to/viewerjs/css/viewer.css">
</head>
<body>
    <iframe src="path/to/viewerjs/#../path/to/your/file.pdf" style="width: 100%; height: 100vh;" frameborder="0"></iframe>
</body>
</html>

```

4. Using PHP Libraries for PDF Generation
If you are more interested in generating PDFs with PHP, consider using libraries like:

<br>TCPDF: A PHP class for generating PDF documents. TCPDF Documentation
<br>FPDF: Another popular PHP class for creating PDF files. FPDF Documentation
<br>These libraries don’t view PDFs but can generate them for you. Combine them with PDF.js or other viewers to display PDFs in your web application.

<br>Summary
<br>PDF.js: Great for displaying PDFs directly in the browser.
<br>PDFObject: Simple and lightweight for embedding PDFs.
<br>ViewerJS: Another option based on PDF.js.
<br>PHP Libraries (TCPDF, FPDF): For generating PDFs to be viewed using the above tools.
<br>Choose the method based on whether you need to display or generate PDFs and how you want to integrate the solution into your PHP-based web application.
