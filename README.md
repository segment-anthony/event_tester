<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Segment Event Tracking</title>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.8.0/styles/default.min.css" rel="stylesheet">
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            height: 100vh;
            background-color: #f4f4f4;
            margin: 0;
        }
        .container {
            display: flex;
            flex-direction: row;
            justify-content: space-between;
            width: 1000px;
        }
        .form-section {
            width: 45%;
            padding: 20px;
            background: white;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        .json-section {
            width: 45%;
            padding: 20px;
            background: white;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        button {
            background-color: #007BFF;
            color: white;
            border: none;
            padding: 10px 20px;
            margin: 10px;
            font-size: 16px;
            cursor: pointer;
            border-radius: 5px;
        }
        button:hover {
            background-color: #0056b3;
        }
        
        form {
            margin-top: 20px;
        }
        input[type="email"], input[type="text"] {
            padding: 8px;
            width: 250px;
            border: 1px solid #ccc;
            border-radius: 4px;
            margin-bottom: 10px;
        }
        input[type="submit"] {
            background-color: #28a745;
            color: white;
            padding: 10px 15px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        input[type="submit"]:hover {
            background-color: #218838;
        }
        #jsonPreviewTrack, #jsonPreviewIdentify {
            padding: 20px;
            border: 1px solid #ddd;
            border-radius: 5px;
            background-color: #2e2e2e;
            color: #f8f8f2;
            width: 100%;
            height: 300px;
            overflow-y: auto;
            font-family: monospace;
            white-space: pre-wrap;
            word-wrap: break-word;
            box-sizing: border-box;
            margin-top: 20px;
        }
        .row {
            display: flex;
            margin-bottom: 10px;
            justify-content: space-between;
        }
        .row input {
            width: 45%;
            margin-right: 10px;
        }

        /* Manually applying color for JSON keys */
        .json-key {
            color: #ffcc00 !important; /* Bright yellow for keys */
            font-weight: bold;
        }
        .json-value {
            color: #80ff80 !important; /* Light green for values */
        }

        /* Ensure json keys are displayed in bright yellow */
        pre {
            color: white !important;
        }

        .track-event-button, .submit-identify-button {
            background-color: #28a745; /* Same color as Submit Identify */
            color: white;
            border: none;
            padding: 10px 20px;
            margin: 10px;
            font-size: 16px;
            cursor: pointer;
            border-radius: 5px;
        }
        .track-event-button:hover, .submit-identify-button:hover {
            background-color: #218838; /* Same hover color as Submit Identify */
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="form-section">
            <h1>Segment Event Tracking</h1>
            <div>
                <h3>Track Event</h3>
                <input type="text" id="eventName" placeholder="Event Name" oninput="updateJsonPreview()"><br>
                <div id="eventPropsContainer"></div>
                <button onclick="addPropertyField()">Add Property</button>
                <button class="track-event-button" onclick="trackEvent()">Track Event</button>
            </div>

            <div>
                <h3>Submit Identify</h3>
                <form id="identifyForm">
                    <input type="text" id="userId" placeholder="User ID (Optional)" oninput="updateJsonPreview()"><br>
                    <input type="email" id="email" placeholder="Email" required oninput="updateJsonPreview()"><br>
                    <div id="identifyPropsContainer"></div>
                    <button type="button" onclick="addIdentifyPropertyField()">Add Trait</button>
                    <input type="submit" class="submit-identify-button" value="Submit Identify">
                </form>
            </div>
        </div>

        <div class="json-section">
            <h2>JSON Preview</h2>
            <div><strong>Track Event</strong></div>
            <pre id="jsonPreviewTrack"></pre>
            <div><strong>Identify</strong></div>
            <pre id="jsonPreviewIdentify"></pre>
        </div>
    </div>

    <!-- Segment Analytics Snippet -->
<script>
  !function(){var i="analytics",analytics=window[i]=window[i]||[];if(!analytics.initialize)if(analytics.invoked)window.console&&console.error&&console.error("Segment snippet included twice.");else{analytics.invoked=!0;analytics.methods=["trackSubmit","trackClick","trackLink","trackForm","pageview","identify","reset","group","track","ready","alias","debug","page","screen","once","off","on","addSourceMiddleware","addIntegrationMiddleware","setAnonymousId","addDestinationMiddleware","register"];analytics.factory=function(e){return function(){if(window[i].initialized)return window[i][e].apply(window[i],arguments);var n=Array.prototype.slice.call(arguments);if(["track","screen","alias","group","page","identify"].indexOf(e)>-1){var c=document.querySelector("link[rel='canonical']");n.push({__t:"bpc",c:c&&c.getAttribute("href")||void 0,p:location.pathname,u:location.href,s:location.search,t:document.title,r:document.referrer})}n.unshift(e);analytics.push(n);return analytics}};for(var n=0;n<analytics.methods.length;n++){var key=analytics.methods[n];analytics[key]=analytics.factory(key)}analytics.load=function(key,n){var t=document.createElement("script");t.type="text/javascript";t.async=!0;t.setAttribute("data-global-segment-analytics-key",i);t.src="https://cdn.segment.com/analytics.js/v1/" + key + "/analytics.min.js";var r=document.getElementsByTagName("script")[0];r.parentNode.insertBefore(t,r);analytics._loadOptions=n};analytics._writeKey="N33kFgfjctRZb3GcXfJPWiU7o8gCsDev";;analytics.SNIPPET_VERSION="5.2.0";
  analytics.load("N33kFgfjctRZb3GcXfJPWiU7o8gCsDev");
  analytics.page();
  }}();
</script>

    <script>
        function addPropertyField() {
            var container = document.getElementById('eventPropsContainer');
            var div = document.createElement('div');
            div.classList.add('row');
            div.innerHTML = '<input type="text" placeholder="Property" class="prop-key" oninput="updateJsonPreview()"> <input type="text" placeholder="Value" class="prop-value" oninput="updateJsonPreview()">';
            container.appendChild(div);
            updateJsonPreview();
        }

        function addIdentifyPropertyField() {
            var container = document.getElementById('identifyPropsContainer');
            var div = document.createElement('div');
            div.classList.add('row');
            div.innerHTML = '<input type="text" placeholder="Trait" class="identify-key" oninput="updateJsonPreview()"> <input type="text" placeholder="Value" class="identify-value" oninput="updateJsonPreview()">';
            container.appendChild(div);
            updateJsonPreview();
        }

        function updateJsonPreview() {
            var eventName = document.getElementById('eventName').value.trim();
            var eventProps = {};
            var keys = document.querySelectorAll('.prop-key');
            var values = document.querySelectorAll('.prop-value');
            for (var i = 0; i < keys.length; i++) {
                var key = keys[i].value.trim();
                var value = values[i].value.trim();
                if (key) {
                    eventProps[key] = value;
                }
            }

            var eventPayload = {
                "type": "track",
                "event": eventName,
                "properties": eventProps
            };

            var userId = document.getElementById('userId').value.trim();
            var email = document.getElementById('email').value.trim();
            var traits = { email: email };
            var identifyKeys = document.querySelectorAll('.identify-key');
            var identifyValues = document.querySelectorAll('.identify-value');
            for (var i = 0; i < identifyKeys.length; i++) {
                var key = identifyKeys[i].value.trim();
                var value = identifyValues[i].value.trim();
                if (key) {
                    traits[key] = value;
                }
            }

            var identifyPayload = {
                "type": "identify",
                "traits": traits,
                "userId": userId
            };

            var jsonPreviewTrack = document.getElementById('jsonPreviewTrack');
            var jsonPreviewIdentify = document.getElementById('jsonPreviewIdentify');

            var formattedJsonTrack = `Track Event:
{
  "type": "track",
  "event": "${eventName}",
  "properties": {`;

            // Ensure proper indentation for the first property
            var eventPropsFormatted = Object.keys(eventProps).map((key, index) => {
                return `    "${key}": "${eventProps[key]}"`;
            }).join(",\n");

            formattedJsonTrack += `\n${eventPropsFormatted}\n  }`;

            var formattedJsonIdentify = `Identify:
{
  "type": "identify",
  "traits": {`;

            // Ensure email is only once
            var traitsFormatted = Object.keys(traits).map((key, index) => {
                return `    "${key}": "${traits[key]}"`;
            }).join(",\n");

            formattedJsonIdentify += `\n${traitsFormatted}\n  },\n  "userId": "${userId}"\n}`;

            // Apply custom styling to the JSON keys and values
            formattedJsonTrack = formattedJsonTrack
                .replace(/"([^"]+)":/g, '"<span class="json-key">$1</span>":')  // Apply custom class for keys
                .replace(/: "(.*?)"/g, ': "<span class="json-value">$1</span>"'); // Apply custom class for values

            formattedJsonIdentify = formattedJsonIdentify
                .replace(/"([^"]+)":/g, '"<span class="json-key">$1</span>":')  // Apply custom class for keys
                .replace(/: "(.*?)"/g, ': "<span class="json-value">$1</span>"'); // Apply custom class for values

            jsonPreviewTrack.innerHTML = formattedJsonTrack;
            jsonPreviewIdentify.innerHTML = formattedJsonIdentify;
        }

        document.getElementById('identifyForm').addEventListener('submit', function(e) {
            e.preventDefault();
            var userId = document.getElementById('userId').value.trim();
            var email = document.getElementById('email').value.trim();
            var traits = { email: email };
            var keys = document.querySelectorAll('.identify-key');
            var values = document.querySelectorAll('.identify-value');
            for (var i = 0; i < keys.length; i++) {
                var key = keys[i].value.trim();
                var value = values[i].value.trim();
                if (key) {
                    traits[key] = value;
                }
            }

            if (userId) {
                analytics.identify(userId, traits);
            } else {
                analytics.identify(traits);
            }
            alert('Identify call sent successfully!');
        });

        function trackEvent() {
            var eventName = document.getElementById('eventName').value.trim();
            if (!eventName) {
                alert('Please enter an event name.');
                return;
            }

            var props = {};
            var keys = document.querySelectorAll('.prop-key');
            var values = document.querySelectorAll('.prop-value');
            for (var i = 0; i < keys.length; i++) {
                var key = keys[i].value.trim();
                var value = values[i].value.trim();
                if (key) {
                    props[key] = value;
                }
            }

            analytics.track(eventName, props);
            alert('Event tracked successfully!');
        }
    </script>
</body>
</html>
