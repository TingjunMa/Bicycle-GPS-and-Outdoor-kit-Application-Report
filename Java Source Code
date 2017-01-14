(1)获取GPS坐标
package com.example.thisismybike;
//dingweiSDK
import android.R.string;
import android.app.Activity;
import android.os.Bundle;
import android.os.Handler;
import android.os.Message;

import com.example.thisismybike.R;
import com.baidu.location.BDLocation;
import com.baidu.location.BDLocationListener;
import com.baidu.location.LocationClient;
import com.baidu.location.LocationClientOption;
import com.baidu.location.BDNotifyListener;//假如用到位置提醒功能，需要import该类
//如果使用地理围栏功能，需要import如下类
import com.baidu.location.BDGeofence;
import com.baidu.location.BDLocationStatusCodes;
import com.baidu.location.GeofenceClient;
import com.baidu.location.GeofenceClient.OnAddBDGeofencesResultListener;
import com.baidu.location.GeofenceClient.OnGeofenceTriggerListener;
import com.baidu.location.GeofenceClient.OnRemoveBDGeofencesResultListener;
import com.baidu.location.LocationClientOption.LocationMode;

import android.app.Application;
import android.app.Service;
import android.content.Intent;
import android.content.SharedPreferences;
import android.content.SharedPreferences.Editor;
import android.os.Vibrator;
import android.util.Log;
import android.view.KeyEvent;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;
import android.widget.CheckBox;
import android.widget.ImageButton;
import android.widget.TextView;

import com.baidu.location.BDLocation;
import com.baidu.location.BDLocationListener;
import com.baidu.location.GeofenceClient;
import com.baidu.location.LocationClient;
import com.baidu.mapapi.SDKInitializer;
import com.baidu.mapapi.model.LatLng;
import com.baidu.mapapi.utils.DistanceUtil;

public class Messagemenu extends Activity{
//private Handler handler;
private TextView locationInfoTextView = null;
private LocationClient locationClient = null;
private static final int UPDATE_TIME = 1050;
public double bikelatitude=23.171723;
public double bikelongitude=113.347231;
public double mylatitude=0;
public double mylongitude=0;
public String bikeplacename=null;
public LatLng myPoint;
public LatLng bikePoint;
public TextView DISTANCE;
@Override
protected void onCreate(Bundle savedInstanceState) {
super.onCreate(savedInstanceState);
setContentView(R.layout.message_menu);
SDKInitializer.initialize(getApplicationContext());
locationInfoTextView = (TextView) this.findViewById(R.id.mylocationtext);
SharedPreferences sp=getSharedPreferences("jingwei", MODE_PRIVATE);
String lonstr=sp.getString("bikelongitude", "113.350338");
String latstr=sp.getString("bikelatitude", "23.167386");
String bikeaddr=sp.getString("bikeaddr", "unknown");
bikePoint = new LatLng(Double.valueOf(latstr), Double.valueOf(lonstr));
TextView bikelocation=(TextView)findViewById(R.id.mybiketext);
bikelocation.setText("Latitude :\n"+latstr+"\nLontitude :\n"+lonstr+"\nAddress :\n "+bikeaddr);


//SDKInitializer.initialize(this);
locationClient = new LocationClient(this);
//设置定位条件
LocationClientOption option = new LocationClientOption();
option.setOpenGps(true);        //是否打开GPS
option.setLocationMode(LocationMode.Hight_Accuracy);
option.setCoorType("bd09ll");       //设置返回值的坐标类型。
option.setProdName("LocationDemo"); //设置产品线名称。强烈建议您使用自定义的产品线名称，方便我们以后为您提供更高效准确的定位服务。
option.setScanSpan(UPDATE_TIME);    //设置定时定位的时间间隔。单位毫秒
option.setIsNeedAddress(true);
option.setNeedDeviceDirect(true);
locationClient.setLocOption(option);

final ImageButton streetviewbutton=(ImageButton)findViewById(R.id.surroundingview);
streetviewbutton.setOnClickListener(new OnClickListener() {

@Override
public void onClick(View arg0) {
// TODO Auto-generated method stub
//	streetviewbutton.setBackgroundResource(R.drawable.surroundingview2);
Intent intent =new Intent(Messagemenu.this,Surroundingview.class);
startActivity(intent);
finish();
}
});

final ImageButton openmapbutton=(ImageButton)findViewById(R.id.openthemap);
openmapbutton.setOnClickListener(new OnClickListener() {

@Override
public void onClick(View arg0) {
// TODO Auto-generated method stub
//openmapbutton.setBackgroundResource(R.drawable.opengps2);
Intent intent =new Intent(Messagemenu.this,Openmap.class);
startActivity(intent);
finish();
}
});

//注册位置监听器
locationClient.registerLocationListener(new BDLocationListener() {

@Override
public void onReceiveLocation(final BDLocation location) {
// TODO Auto-generated method stub
if (location == null) {
return;
}
mylatitude=location.getLatitude();
mylongitude=location.getLongitude();
myPoint = new LatLng(mylatitude, mylongitude);
//mylatitude=(int)(location.getLatitude()*1000000);
//mylongitude=(int)(location.getLongitude()*1000000);
final ImageButton storemybike=(ImageButton)findViewById(R.id.storemybike);
storemybike.setOnClickListener(new OnClickListener() {

@Override
public void onClick(View arg0) {
// TODO Auto-generated method stub
//	storemybike.setBackgroundResource(R.drawable.storemybike2);
//	storemybike.setBackgroundResource(R.drawable.storemybike);
bikelatitude=location.getLatitude();
bikelongitude=location.getLongitude();
bikePoint = new LatLng(bikelatitude, bikelongitude);
bikeplacename=location.getAddrStr();
DISTANCE=(TextView)findViewById(R.id.distancetext);
double distance1 = DistanceUtil.getDistance(myPoint, bikePoint);
DISTANCE.setText(distance1+"M");
SharedPreferences sp=getSharedPreferences("jingwei", MODE_PRIVATE);
Editor loceditor=sp.edit();

loceditor.putString("bikelatitude", String.valueOf(bikelatitude));
loceditor.putString("bikelongitude", String.valueOf(bikelongitude));
loceditor.putString("bikeaddr", bikeplacename);
loceditor.commit();
TextView bikelocation=(TextView)findViewById(R.id.mybiketext);
bikelocation.setText("Latitude :\n"+bikelatitude+"\nLontitude :\n"+bikelongitude+"\nAddress :\n "+bikeplacename);
}
});
StringBuffer sb = new StringBuffer(256);
sb.append("Latitude :\n ");
sb.append(location.getLatitude());
sb.append("\nLontitude :\n ");
sb.append(location.getLongitude());
sb.append("\nAddress :\n ");
sb.append(location.getAddrStr());
locationInfoTextView.setText(sb.toString());
DISTANCE=(TextView)findViewById(R.id.distancetext);
double distance1 = DistanceUtil.getDistance(myPoint, bikePoint);
DISTANCE.setText(distance1+"M");
}

});

locationClient.start();
//myPoint = new LatLng(mylatitude, mylongitude);
//bikePoint = new LatLng(bikelatitude, bikelongitude);
/*    double distance1 = DistanceUtil.getDistance(myPoint, bikePoint);
DISTANCE=(TextView)findViewById(R.id.distancetext);
DISTANCE.setText(distance1+"M");
Thread threadgetdistance=new Thread(this);
threadgetdistance.start();
handler=new Handler(){
@Override
public void handleMessage(Message msg){
//DISTANCE=(TextView)findViewById(R.id.distancetext);
DISTANCE.setText(msg.getData().getDouble("D")+"M");
super.handleMessage(msg);
}
};

}
@Override
public void run() {
// TODO Auto-generated method stub
while(!Thread.currentThread().isInterrupted()){
Message m=handler.obtainMessage();
Bundle bundle=new Bundle();
bundle.putDouble("D", DistanceUtil.getDistance(myPoint, bikePoint));
m.setData(bundle);
handler.sendMessage(m);
try{Thread.sleep(500);}catch(InterruptedException e){e.printStackTrace();}
}*/
}
@Override
public boolean onKeyDown(int keyCode, KeyEvent event) {
if (keyCode == KeyEvent.KEYCODE_BACK && event.getRepeatCount() == 0) {
Intent intent=new Intent(Messagemenu.this,MainActivity.class);
startActivity(intent);
finish();
return true;
} else
return super.onKeyDown(keyCode, event);
}

}
 

（2）调用百度地图
package com.example.thisismybike;

import android.app.Activity;
import android.content.Intent;
import android.content.SharedPreferences;
import android.os.Bundle;
import android.view.KeyEvent;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;
import android.widget.RadioGroup;
import android.widget.RadioGroup.OnCheckedChangeListener;

import com.baidu.location.BDLocation;
import com.baidu.location.BDLocationListener;
import com.baidu.location.LocationClient;
import com.baidu.location.LocationClientOption;
import com.baidu.mapapi.map.BaiduMap;
import com.baidu.mapapi.map.BitmapDescriptor;
import com.baidu.mapapi.map.BitmapDescriptorFactory;
import com.baidu.mapapi.map.MapStatusUpdate;
import com.baidu.mapapi.map.MapStatusUpdateFactory;
import com.baidu.mapapi.map.MapView;
import com.baidu.mapapi.map.MarkerOptions;
import com.baidu.mapapi.map.MyLocationConfiguration;
import com.baidu.mapapi.map.MyLocationConfiguration.LocationMode;
import com.baidu.mapapi.map.MyLocationData;
import com.baidu.mapapi.map.OverlayOptions;
import com.baidu.mapapi.model.LatLng;
import com.baidu.mapapi.overlayutil.PoiOverlay;
import com.baidu.mapapi.search.core.PoiInfo;
import com.baidu.mapapi.search.poi.PoiDetailSearchOption;
import com.baidu.mapapi.search.poi.PoiNearbySearchOption;
import com.baidu.mapapi.search.poi.PoiSearch;

/**
 * 此demo用来展示如何结合定位SDK实现定位，并使用MyLocationOverlay绘制定位位置 同时展示如何使用自定义图标绘制并点击时弹出泡泡
 * 
 */
public class Openmap extends Activity {

	// 定位相关
	LocationClient mLocClient;
	public MyLocationListenner myListener = new MyLocationListenner();
	private LocationMode mCurrentMode;
	BitmapDescriptor mCurrentMarker;

	MapView mMapView;
	BaiduMap mBaiduMap;

	// UI相关


	boolean isFirstLoc = true;// 是否首次定位
	
	private PoiSearch mPoiSearch = null;

	@Override
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.mymap);
		mCurrentMarker = null;
		mCurrentMode = LocationMode.COMPASS;
	
		// 地图初始化
		mMapView = (MapView) findViewById(R.id.bmapView);
		mBaiduMap = mMapView.getMap();
		mBaiduMap
		.setMyLocationConfigeration(new MyLocationConfiguration(
				mCurrentMode, true, mCurrentMarker));
		// 开启定位图层
		mBaiduMap.setMyLocationEnabled(true);
		// 定位初始化
		mLocClient = new LocationClient(this);
		mLocClient.registerLocationListener(myListener);
		LocationClientOption option = new LocationClientOption();
		option.setOpenGps(true);// 打开gps
		option.setCoorType("bd09ll"); // 设置坐标类型
		option.setScanSpan(1000);
		option.setNeedDeviceDirect(true);
		mLocClient.setLocOption(option);
		mLocClient.start();
		//定义bikeMaker坐标点  
    	SharedPreferences sp=getSharedPreferences("jingwei", MODE_PRIVATE);
    	String lonstr=sp.getString("bikelongitude", "113.350338");
    	String latstr=sp.getString("bikelatitude", "23.167386");
		LatLng point = new LatLng(Double.valueOf(latstr), Double.valueOf(lonstr));  
		//构建Marker图标  
		BitmapDescriptor bitmap = BitmapDescriptorFactory  
		    .fromResource(R.drawable.icon_bike);  
		//构建MarkerOption，用于在地图上添加Marker  
		OverlayOptions overlaybike = new MarkerOptions()  
		    .position(point)  
		    .icon(bitmap);  
		//在地图上添加Marker，并显示  
		mBaiduMap.addOverlay(overlaybike);
		
		//POI nearby bike
		/*mPoiSearch.searchNearby(new PoiNearbySearchOption().radius(5000).keyword("自行车维修").location(point));
		PoiOverlay overlay = new MyPoiOverlay(mBaiduMap);
		mBaiduMap.setOnMarkerClickListener(overlay);
		overlay.setData();
		overlay.addToMap();
		overlay.zoomToSpan();*/
	}

	/**
	 * 定位SDK监听函数
	 */
	public class MyLocationListenner implements BDLocationListener {

		@Override
		public void onReceiveLocation(BDLocation location) {
			// map view 销毁后不在处理新接收的位置
			if (location == null || mMapView == null)
				return;
			MyLocationData locData = new MyLocationData.Builder()
					.accuracy(location.getRadius())
					// 此处设置开发者获取到的方向信息，顺时针0-360
					.direction(location.getDirection()).latitude(location.getLatitude())
					.longitude(location.getLongitude()).build();
			mBaiduMap.setMyLocationData(locData);
			
			
			
			if (isFirstLoc) {
				isFirstLoc = false;
				LatLng ll = new LatLng(location.getLatitude(),
						location.getLongitude());
				MapStatusUpdate u = MapStatusUpdateFactory.newLatLng(ll);
				mBaiduMap.animateMapStatus(u);
			}
		}

		public void onReceivePoi(BDLocation poiLocation) {
		}
	}
	
	//POI new class
	private class MyPoiOverlay extends PoiOverlay {

		public MyPoiOverlay(BaiduMap baiduMap) {
			super(baiduMap);
		}

		@Override
		public boolean onPoiClick(int index) {
			super.onPoiClick(index);
			PoiInfo poi = getPoiResult().getAllPoi().get(index);
			// if (poi.hasCaterDetails) {
				mPoiSearch.searchPoiDetail((new PoiDetailSearchOption())
						.poiUid(poi.uid));
			// }
			return true;
		}
	}

	@Override
	protected void onPause() {
		mMapView.onPause();
		super.onPause();
	}

	@Override
	protected void onResume() {
		mMapView.onResume();
		super.onResume();
	}

	@Override
	protected void onDestroy() {
		// 退出时销毁定位
		mLocClient.stop();
		// 关闭定位图层
		mBaiduMap.setMyLocationEnabled(false);
		mMapView.onDestroy();
		mMapView = null;
		super.onDestroy();
	}
	@Override  
    public boolean onKeyDown(int keyCode, KeyEvent event) {  
        if (keyCode == KeyEvent.KEYCODE_BACK && event.getRepeatCount() == 0) {  
              Intent intent=new Intent(Openmap.this,Messagemenu.class);
              startActivity(intent);
              finish();
            return true;  
        } else  
            return super.onKeyDown(keyCode, event);  
    }  
}

 

（3）调用百度街景

package com.example.thisismybike;

import android.R.string;
import android.app.Activity;
import android.content.Intent;
import android.content.SharedPreferences;
import android.os.Bundle;
import android.view.KeyEvent;

import com.baidu.lbsapi.panoramaview.*;  
import com.baidu.lbsapi.BMapManager;  
import com.baidu.mapapi.SDKInitializer;
import com.baidu.pplatform.comapi.basestruct.GeoPoint;
public class Surroundingview extends Activity implements PanoramaViewListener {
	private PanoramaView mPanoView;
    @Override  
    public void onCreate(Bundle savedInstanceState){ 
        super.onCreate(savedInstanceState);
        SDKInitializer.initialize(getApplicationContext()); 
    	DemoApplication app = (DemoApplication) this.getApplication();  
    	if (app.mBMapManager == null) {  
    	    app.mBMapManager = new BMapManager(app);  
    	 
    	    app.mBMapManager.init(new DemoApplication.MyGeneralListener());  
    	}  
    	setContentView(R.layout.streetview);
    	mPanoView = (PanoramaView) findViewById(R.id.panorama);
    	mPanoView.setPanoramaImageLevel(5);
    	//mPanoView.setPanoramaViewListener(this);
    	SharedPreferences sp=getSharedPreferences("jingwei", MODE_PRIVATE);
    	String lonstr=sp.getString("bikelongitude", "113.350338");
    	String latstr=sp.getString("bikelatitude", "23.167386");
    	mPanoView.setPanorama(Double.valueOf(lonstr),Double.valueOf(latstr));
    	
    }
    @Override  
    protected void onPause() {  
        super.onPause();  
        mPanoView.onPause();  
    }  
     
    @Override  
    protected void onResume() {  
        super.onResume();  
        mPanoView.onResume();  
    }  
     
    @Override  
    protected void onDestroy() {  
        mPanoView.destroy();  
        super.onDestroy();  
    }
	@Override
	public void onLoadPanoramBegin() {
		// TODO Auto-generated method stub
		
	}
	@Override
	public void onLoadPanoramaDesc() {
		// TODO Auto-generated method stub
		
	}
	@Override
	public void onLoadPanoramaEnd() {
		// TODO Auto-generated method stub
		
	}
	@Override
	public void onLoadPanoramaError() {
		// TODO Auto-generated method stub
		
	}
	@Override  
    public boolean onKeyDown(int keyCode, KeyEvent event) {  
        if (keyCode == KeyEvent.KEYCODE_BACK && event.getRepeatCount() == 0) {  
              Intent intent=new Intent(Surroundingview.this,Messagemenu.class);
              startActivity(intent);
              finish();
            return true;  
        } else  
            return super.onKeyDown(keyCode, event);  
    } 
}
 
（4）接收SMS并读取其中GPS坐标

package com.example.thisismybike;

import android.app.Activity;
import android.content.BroadcastReceiver;
import android.content.Context;
import android.content.Intent;
import android.content.SharedPreferences;
import android.content.SharedPreferences.Editor;
import android.os.Bundle;
import android.telephony.SmsMessage;
import android.widget.TextView;

public class SMSBroadcastReceiver extends BroadcastReceiver {
	private static final String ACTION = "android.provider.Telephony.SMS_RECEIVED";

	@Override
	public void onReceive(Context context, Intent intent) {
		
		if (intent.getAction().equals(ACTION)) {
			StringBuffer SMSAddress = new StringBuffer();
			StringBuffer SMSContent = new StringBuffer();
			Bundle bundle = intent.getExtras();
			if (bundle != null) {
				Object[] pdusObjects = (Object[]) bundle.get("pdus");
				SmsMessage[] messages = new SmsMessage[pdusObjects.length];
				for (int i = 0; i < pdusObjects.length; i++) {
					messages[i] = SmsMessage
							.createFromPdu((byte[]) pdusObjects[i]);
				}
				for (SmsMessage message : messages) {
					SMSAddress.append(message.getDisplayOriginatingAddress());
					SMSContent.append(message.getDisplayMessageBody());
					{
						String [] temp = null;  
						String SMStemp=String.valueOf(SMSContent);
					    temp = SMStemp.split(",");
					    String lat=temp[0];
					    String lon=temp[1];
						SharedPreferences sp= context.getSharedPreferences("jingwei",Activity.MODE_PRIVATE);
        				Editor loceditor=sp.edit();
        				loceditor.putString("bikelatitude2", lat);
        				loceditor.putString("bikelongitude2", lon);
        				loceditor.commit();
					}
				}
			}
		}
	}

}
六、其他代码注释
MainActivity.java:
package com.example.thisismybike;

import android.app.Activity;
import android.content.Intent;
import android.os.Bundle;
import android.view.Menu;
import android.view.MenuItem;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.ImageButton;

import com.example.thisismybike.R;
public class MainActivity extends Activity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        final ImageButton ezmode=(ImageButton)findViewById(R.id.ezmode);
        ezmode.setOnClickListener(new OnClickListener() {
			
			@Override
			public void onClick(View arg0) {
				// TODO Auto-generated method stub
				//ezmode.setBackgroundResource(R.drawable.ezmode2);
				Intent intent =new Intent(MainActivity.this,Messagemenu.class);
				startActivity(intent);
				finish();
			}
		});
        final ImageButton exmod=(ImageButton)findViewById(R.id.exmode);
        exmod.setOnClickListener(new OnClickListener() {
			
			@Override
			public void onClick(View arg0) {
				// TODO Auto-generated method stub
				//exmod.setBackgroundResource(R.drawable.exmod2);
				Intent intent =new Intent(MainActivity.this,Exmod.class);
				startActivity(intent);
				finish();
			}
		});
        
        final ImageButton exit=(ImageButton)findViewById(R.id.exit);
        exit.setOnClickListener(new OnClickListener() {
			
			@Override
			public void onClick(View arg0) {
				// TODO Auto-generated method stub
			//	exmod.setBackgroundResource(R.drawable.exit2);
				finish();
			}
		});
        
        
        final ImageButton instru=(ImageButton)findViewById(R.id.instruction);
        instru.setOnClickListener(new OnClickListener() {
			
			@Override
			public void onClick(View arg0) {
				// TODO Auto-generated method stub
				Intent intent =new Intent(MainActivity.this,Instruction.class);
				startActivity(intent);
				finish();
			}
		});
        
    }
}

EX MODE.java
package com.example.thisismybike;
import android.app.Activity;
import android.os.Bundle;




import com.example.thisismybike.R;

import android.content.Intent;
import android.content.SharedPreferences;
import android.content.SharedPreferences.Editor;
import android.view.KeyEvent;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.ImageButton;
import android.widget.TextView;

public class Exmod extends Activity {

	
 @Override
 protected void onCreate(Bundle savedInstanceState) {
     super.onCreate(savedInstanceState);
     setContentView(R.layout.exmod_menu);
    
     
     final ImageButton back2ez=(ImageButton)findViewById(R.id.backtoez);
     back2ez.setOnClickListener(new OnClickListener() {
			
			@Override
			public void onClick(View arg0) {
				// TODO Auto-generated method stub
				//back2ez.setBackgroundResource(R.drawable.backtoezmod2);
				Intent intent =new Intent(Exmod.this,Messagemenu.class);
				startActivity(intent);
				finish();
			}
		});
     
     final ImageButton opengps=(ImageButton)findViewById(R.id.opengps);
     opengps.setOnClickListener(new OnClickListener() {
			
			@Override
			public void onClick(View arg0) {
				// TODO Auto-generated method stub
			//	opengps.setBackgroundResource(R.drawable.opengps2);
			//	opengps.setBackgroundResource(R.drawable.opengps);
				SharedPreferences sp=getSharedPreferences("jingwei", MODE_PRIVATE);
		    	String lonstr=sp.getString("bikelongitude2", "113.350338");
		    	String latstr=sp.getString("bikelatitude2", "23.167386");
		    	String bikeaddr=sp.getString("bikeaddr", "unknown");
				Editor loceditor=sp.edit();
				loceditor.putString("bikelatitude",latstr);
				loceditor.putString("bikelongitude", lonstr);
				loceditor.putString("bikeaddr", bikeaddr);
				loceditor.commit();
				TextView refreshtext=(TextView)findViewById(R.id.refresh);
				refreshtext.setText("已刷新！当前单车位于\n"+"latitude:"+latstr+"\nlongitude:"+lonstr);
				
			}
		});
    
 }
 
	@Override  
    public boolean onKeyDown(int keyCode, KeyEvent event) {  
        if (keyCode == KeyEvent.KEYCODE_BACK && event.getRepeatCount() == 0) {  
              Intent intent=new Intent(Exmod.this,MainActivity.class);
              startActivity(intent);
              finish();
            return true;  
        } else  
            return super.onKeyDown(keyCode, event);  
    } 

}

Instruction.java
package com.example.thisismybike;

import android.app.Activity;
import android.content.Intent;
import android.os.Bundle;
import android.view.KeyEvent;

public class Instruction extends Activity {
@Override
protected void onCreate(Bundle savedInstanceState) {
	// TODO Auto-generated method stub
	super.onCreate(savedInstanceState);
	setContentView(R.layout.instructionmenu);
}

@Override  
public boolean onKeyDown(int keyCode, KeyEvent event) {  
    if (keyCode == KeyEvent.KEYCODE_BACK && event.getRepeatCount() == 0) {  
          Intent intent=new Intent(Instruction.this,MainActivity.class);
          startActivity(intent);
          finish();
        return true;  
    } else  
        return super.onKeyDown(keyCode, event);  
} 
}


