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

**Test Script**

package webSocket.DateandTimeSatmp;

import org.json.JSONObject;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.logging.LogEntries;
import org.openqa.selenium.logging.LogEntry;
import org.openqa.selenium.logging.LogType;
import org.openqa.selenium.logging.LoggingPreferences;
import org.openqa.selenium.remote.CapabilityType;
import org.openqa.selenium.remote.DesiredCapabilities;

import java.util.Date;
import java.util.function.Consumer;
import java.util.logging.Level;

public class DateandTimestamp {

	private static WebDriver driver;
	public static void main(String[] args) throws InterruptedException{
		
    	System.setProperty("webdriver.chrome.driver","/Applications/Eclipse/xyz/chromedriver");
        
        // To enable performance log for collecting network events
        LoggingPreferences loggingprefs = new LoggingPreferences();
        loggingprefs.enable(LogType.PERFORMANCE, Level.ALL);
        DesiredCapabilities cap = new DesiredCapabilities().chrome();
        cap.setCapability(CapabilityType.LOGGING_PREFS, loggingprefs);

        //To launch Google Chrome 
       driver = new ChromeDriver(cap);
       driver.navigate().to("file:///Users/mariappanvijayavelan/count.html");
       Thread.sleep(5000);
       LogEntries logEntries = driver.manage().logs().get(LogType.PERFORMANCE);
        
       //To read System's DateandTimestamp  
       final Date currentDate = new Date();
        System.out.println("current system date and time "+ currentDate);
       
        //To close Google Chrome
        driver.close();
        driver.quit();
        
        //To read websocket DateandTimeStamp  
        logEntries.forEach(new Consumer<LogEntry>() {
		public void accept(LogEntry entry) {
			    JSONObject messageJSON = new JSONObject(entry.getMessage());
			    String method = messageJSON.getJSONObject("message").getString("method");
			    if(method.equalsIgnoreCase("Network.webSocketFrameReceived"))
			    {
			        System.out.println("System Date and Time: "+ currentDate);
			    	System.out.println("Message Received: " + messageJSON.getJSONObject("message").getJSONObject("params").getJSONObject("response").getString("payloadData"));
			    	//To Compare Websocket DateandTimeStamp with system's DateandTimeStamp
			        if (currentDate.equals(messageJSON.getJSONObject("message").getJSONObject("params").getJSONObject("response").getString("payloadData")))
			        {
			        	System.out.println("websocket sevice Date and time is equal to system time");
			        }
			        else{
			        	
			        	System.out.println("Date and Time is Mismatch");
			        }
			    }
			 }
		});
       
}    
}
