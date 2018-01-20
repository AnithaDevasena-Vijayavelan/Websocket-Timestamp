# Websocket-DateandTimestamp
Comparsion of Websocket dateandtimestamp with system's current DateandTime

**Test Description:**

**Test Environment Setup:**
1) Install websocket in MacOS. 
2) Use "websocket-stub.sh" shell script and run the websocket server using "websocketd --port=8080 ./websocket-stub.sh" command 
3) use "websocket-client.js" to create Count.html(File is given below. To check this html, use Google chrome browser and open the file Count.html)
"<script>
  // helper function: log message to screen
  function log(msg) {
    document.getElementById('log').textContent += msg + '\n';
  }
  // setup websocket with callbacks
  var ws = new WebSocket('ws://localhost:8080/');
  ws.onopen = function() {
    log('CONNECT');
  };
  ws.onclose = function() {
    log('DISCONNECT');
  };
  ws.onmessage = function(event) {
    log('Current time is: ' + event.data);
  };
</script>"
 
**Test Steps:**
1) Create Maven Project and write selenium test script to read websocket dateandtimestamp and compare with system's current dateandtimestamp.
