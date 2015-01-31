                                                 #NIKHIL CHOUDHARY
                                                 #VIT UNIVERSITY,Vellore,TN,INDIA.
                                                 #3rd Yr
# BluetoothCallApp
##Free calling using Bluetooth!!! 
##Implementation
##Codes

1.	MainActivity.java

package com.example.bluetoothcall;

import java.util.ArrayList;
import java.util.List;
import java.util.Set;

import android.os.Bundle;
import android.app.Activity;
import android.bluetooth.BluetoothAdapter;
import android.bluetooth.BluetoothDevice;
import android.content.Intent;
import android.view.Menu;
import android.view.View;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.ListAdapter;
import android.widget.ListView;
import android.widget.Toast;
import android.util.Log;
import android.net.Uri;
import android.widget.Button;
import android.telephony.SmsManager;
import android.widget.EditText;

public class MainActivity extends Activity {

   private Button On,Off,Visible,list;
   private BluetoothAdapter BA;
   private Set<BluetoothDevice>pairedDevices;
   private ListView lv;
   
   Button sendBtn;
   EditText txtphoneNo;
   EditText txtMessage;
   @Override
   protected void onCreate(Bundle savedInstanceState) {
      super.onCreate(savedInstanceState);
      setContentView(R.layout.activity_main);
      On = (Button)findViewById(R.id.button1);
      Off = (Button)findViewById(R.id.button2);
      Visible = (Button)findViewById(R.id.button3);
      list = (Button)findViewById(R.id.button4);

      lv = (ListView)findViewById(R.id.listView1);

      BA = BluetoothAdapter.getDefaultAdapter();
      Button startBtn = (Button) findViewById(R.id.makeCall);
      startBtn.setOnClickListener(new View.OnClickListener() {
         public void onClick(View view) {
         makeCall();
         
      }
   });
      sendBtn = (Button) findViewById(R.id.btnSendSMS);
      txtphoneNo = (EditText) findViewById(R.id.editTextPhoneNo);
      txtMessage = (EditText) findViewById(R.id.editTextSMS);

      sendBtn.setOnClickListener(new View.OnClickListener() {
         public void onClick(View view) {
            sendSMSMessage();
         }
      });

   }
   
   protected void makeCall() {
	      Log.i("makeCall", "");

	      //Intent phoneIntent = new Intent(Intent.ACTION_DIAL);
	    //Intent phoneIntent = new Intent(Intent.ACTION_CALL);
	      Intent phoneIntent = new Intent(Intent.ACTION_CALL_BUTTON);
	     // phoneIntent.setData(Uri.parse("tel:91-800-001-0101"));

	      try {
	         startActivity(phoneIntent);
	         finish();
	         Log.i("Finished making a call...", "");
	      } catch (android.content.ActivityNotFoundException ex) {
	         Toast.makeText(MainActivity.this, 
	         "Call faild, please try again later.", Toast.LENGTH_SHORT).show();
	      }
	   }
   protected void sendSMSMessage() {
	      Log.i("Send SMS", "");

	      String phoneNo = txtphoneNo.getText().toString();
	      String message = txtMessage.getText().toString();

	      try {
	         SmsManager smsManager = SmsManager.getDefault();
	         smsManager.sendTextMessage(phoneNo, null, message, null, null);
	         Toast.makeText(getApplicationContext(), "SMS sent.",
	         Toast.LENGTH_LONG).show();
	      } catch (Exception e) {
	         Toast.makeText(getApplicationContext(),
	         "SMS faild, please try again.",
	         Toast.LENGTH_LONG).show();
	         e.printStackTrace();
	      }
	   }
   public void on(View view){
      if (!BA.isEnabled()) {
         Intent turnOn = new Intent(BluetoothAdapter.ACTION_REQUEST_ENABLE);
         startActivityForResult(turnOn, 0);
         Toast.makeText(getApplicationContext(),"Turned on" 
         ,Toast.LENGTH_LONG).show();
      }
      else{
         Toast.makeText(getApplicationContext(),"Already on",
         Toast.LENGTH_LONG).show();
         }
   }
   public void list(View view){
      pairedDevices = BA.getBondedDevices();

      ArrayList list = new ArrayList();
      for(BluetoothDevice bt : pairedDevices)
         list.add(bt.getName());

      Toast.makeText(getApplicationContext(),"Showing Paired Devices",
      Toast.LENGTH_SHORT).show();
      final ArrayAdapter adapter = new ArrayAdapter
      (this,android.R.layout.simple_list_item_1, list);
      lv.setAdapter(adapter);

   }
   public void off(View view){
      BA.disable();
      Toast.makeText(getApplicationContext(),"Turned off" ,
      Toast.LENGTH_LONG).show();
   }
   public void visible(View view){
      Intent getVisible = new Intent(BluetoothAdapter.
      ACTION_REQUEST_DISCOVERABLE);
      startActivityForResult(getVisible, 0);

   }
   @Override
   public boolean onCreateOptionsMenu(Menu menu) {
      // Inflate the menu; this adds items to the action bar if it is present.
      getMenuInflater().inflate(R.menu.main, menu);
      return true;
   }

2.	Activity_main.xml

<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context=".MainActivity" >

   <ScrollView
      android:id="@+id/scrollView1"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:layout_alignParentBottom="true"
      android:layout_alignParentLeft="true"
      android:layout_alignParentRight="true"
      android:layout_alignParentTop="true" >

   <LinearLayout
      android:layout_width="match_parent"
      android:layout_height="match_parent"
      android:orientation="vertical" >
    

   <TextView
      android:id="@+id/textView1"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:text="@string/app_name"
      android:textAppearance="?android:attr/textAppearanceLarge" />

   <Button
      android:id="@+id/button1"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:onClick="on"
      android:text="@string/on" />

   <Button
      android:id="@+id/button2"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:onClick="visible"
      android:text="@string/Visible" />

   <Button
      android:id="@+id/button3"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:onClick="list"
      android:text="@string/List" />

   <Button
      android:id="@+id/button4"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:onClick="off"
      android:text="@string/off" />
   
   <Button 
       android:id="@+id/makeCall"
   android:layout_width="fill_parent"
   android:layout_height="wrap_content"
   android:text="@string/make_call"/>
   <TextView
   android:id="@+id/textViewPhoneNo"
   android:layout_width="wrap_content"
   android:layout_height="wrap_content"
   android:text="@string/phone_label" />

   <EditText
   android:id="@+id/editTextPhoneNo"
   android:layout_width="fill_parent"
   android:layout_height="wrap_content"
   android:inputType="phone"/>

   <TextView
   android:id="@+id/textViewMessage"
   android:layout_width="wrap_content"
   android:layout_height="wrap_content"
   android:text="@string/sms_label" />

   <EditText
   android:id="@+id/editTextSMS"
   android:layout_width="fill_parent"
   android:layout_height="wrap_content"
   android:inputType="textMultiLine"/>

   <Button
       android:id="@+id/btnSendSMS"
       android:layout_width="fill_parent"
       android:layout_height="match_parent"
       android:text="@string/send_sms_label" />

   <ListView
      android:id="@+id/listView1"
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:visibility="visible" >

   </ListView>

   </LinearLayout>
</ScrollView>

</RelativeLayout

3.	Strings.xml

<?xml version="1.0" encoding="utf-8"?>
<resources>

   <string name="app_name">BluetoothCall</string>
   <string name="action_settings">Settings</string>
   <string name="hello_world">Hello world!</string>
   <string name="on">Turn On</string>
   <string name="off">Turn Off</string>
   <string name="Visible">Get Visible</string>
   <string name="List">List Devices</string>
   <string name="make_call">Call</string>
   <string name="phone_label">Enter Phone Number:</string>
    <string name="sms_label">Enter SMS Message:</string>
    <string name="send_sms_label">Send SMS</string>
    

</resources>







4.BluetoothCall.Manifest

<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.bluetoothcall"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-sdk
        android:minSdkVersion="8"
        android:targetSdkVersion="18" />

    <application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
        <activity
            android:name="com.example.bluetoothcall.MainActivity"
            android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
