# Dharani
#include &lt;SoftwareSerial.h&gt;
#include &quot;nodemcu_arduino_pins.h&quot;
#define   MESH_PREFIX            &quot;Mesh&quot;
#define   MESH_PASSWORD          &quot;communication&quot;
#define   MESH_PORT              5555
#define   TASK_RUNNING_INTERVAL  2

void sendMessage() ; 
void senseControl();
Task taskSendMessage( TASK_SECOND * TASK_RUNNING_INTERVAL ,
TASK_FOREVER, &amp;sendMessage );
Task taskSenseControl( 100 , TASK_FOREVER, &amp;senseControl );
long lattitude,longitude; 
 
SoftwareSerial softSerial(D5, D6);
TinyGPS gps; 
int EMERGENCY_BUTTON = D7;
int VIBRATION_SENSOR = D2;
int button_1 = D0;
int button_2 = D1;
String mobile = &quot;6381054601&quot;;
int emergency_alert = 0, vibration_alert = 0;
void setup()
{
  Serial.begin(9600);
  softSerial.begin(9600);
  
  pinMode(EMERGENCY_BUTTON, INPUT);
  pinMode(VIBRATION_SENSOR, INPUT);
  pinMode(button_1, INPUT);
  pinMode(button_2, INPUT);
  mesh.init( MESH_PREFIX, MESH_PASSWORD, MESH_PORT );
  mesh.onReceive(&amp;receivedCallback);
  mesh.onNewConnection(&amp;newConnectionCallback);
  mesh.onChangedConnections(&amp;changedConnectionCallback);
  mesh.onNodeTimeAdjusted(&amp;nodeTimeAdjustedCallback);

  mesh.scheduler.addTask( taskSendMessage );
  taskSendMessage.enable() ;
  
  mesh.scheduler.addTask( taskSenseControl );
  taskSenseControl.enable() ;
  
}
void loop()
{
  mesh.update();
  
  if(softSerial.available())
  {
    if(gps.encode(softSerial.read()))
    {
      gps.get_position(&amp;lattitude,&amp;longitude);
    }
  }
}

void senseControl()
{
    
  if(digitalRead(EMERGENCY_BUTTON) == 1 )
  {
    emergency_alert = 1;
  }
  
  if(digitalRead(VIBRATION_SENSOR) == 1)
  {
    vibration_alert = 1;
  }
  if(digitalRead(button_1) == 1)
  {
    mobile = &quot;6221054601&quot;;
  }
 else if(digitalRead(button_2) == 1)
  {
    mobile = &quot;8940225444&quot;;
  }
  
  lattitude;
  longitude;
  

}
void sendMessage()
{
  String transmitter_id = &quot;c1&quot;;
  String msg;
  
 
  msg += &quot;*&quot; + String(mobile);
  msg += &quot;*&quot; + String(lattitude);
  msg += &quot;*&quot; + String(longitude);
  msg += &quot;*&quot; + String(emergency_alert);
  msg += &quot;*&quot; + String(vibration_alert);
  msg += &quot;#&quot;;
  
  mesh.sendBroadcast( msg );
  Serial.println(&quot;Sent: &quot; + msg);
  taskSendMessage.setInterval( TASK_SECOND * TASK_RUNNING_INTERVAL );
   vibration_alert = 0;
    emergency_alert = 0;
}

void receiveMessage() ; 
void senseControl();
Task taskSendMessage( TASK_SECOND * TASK_RUNNING_INTERVAL ,
TASK_FOREVER, &amp;sendMessage );
Task taskSenseControl( 100 , TASK_FOREVER, &amp;senseControl );
long lattitude,longitude; 
 
SoftwareSerial softSerial(D5, D6);
TinyGPS gps; 
int EMERGENCY_BUTTON = D7;
int VIBRATION_SENSOR = D2;
int button_1 = D0;
int button_2 = D1;
String mobile = &quot;6381054601&quot;;
int emergency_alert = 0, vibration_alert = 0;
void setup()
{
  Serial.begin(9600);
  softSerial.begin(9600);
  
  pinMode(EMERGENCY_BUTTON, INPUT);
  pinMode(VIBRATION_SENSOR, INPUT);
  pinMode(button_1, INPUT);
  pinMode(button_2, INPUT);
  mesh.init( MESH_PREFIX, MESH_PASSWORD, MESH_PORT );
  mesh.onReceive(&amp;receivedCallback);
  mesh.onNewConnection(&amp;newConnectionCallback);
  mesh.onChangedConnections(&amp;changedConnectionCallback);
  mesh.onNodeTimeAdjusted(&amp;nodeTimeAdjustedCallback);

  mesh.scheduler.addTask( taskSendMessage );
  taskSendMessage.enable() ;
  
  mesh.scheduler.addTask( taskSenseControl );
  taskSenseControl.enable() ;
  
}
void loop()
{
  mesh.update();
  
  if(softSerial.available())
  {
    if(gps.encode(softSerial.read()))
    {
      gps.get_position(&amp;lattitude,&amp;longitude);
    }
  }
}

void senseControl()
{
    
  if(digitalRead(EMERGENCY_BUTTON) == 1 )
  {
    emergency_alert = 1;
  }
  
  if(digitalRead(VIBRATION_SENSOR) == 1)
  {
    vibration_alert = 1;
  }
  if(digitalRead(button_1) == 1)
  {
    mobile = &quot;6221054601&quot;;
  }
 else if(digitalRead(button_2) == 1)
  {
    mobile = &quot;8940225444&quot;;
  }
  
  lattitude;
  longitude;
  

}
void sendMessage()
{
  String transmitter_id = &quot;c1&quot;;
  String msg;
  
 
  msg += &quot;*&quot; + String(mobile);
  msg += &quot;*&quot; + String(lattitude);
  msg += &quot;*&quot; + String(longitude);
  msg += &quot;*&quot; + String(emergency_alert);
  msg += &quot;*&quot; + String(vibration_alert);
  msg += &quot;#&quot;;
  
  mesh.sendBroadcast( msg );
  Serial.println(&quot;Sent: &quot; + msg);
  taskSendMessage.setInterval( TASK_SECOND * TASK_RUNNING_INTERVAL );
   vibration_alert = 0;
    emergency_alert = 0;
}
