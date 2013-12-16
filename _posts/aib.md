---
layout: post
title: Why Android Devices Can’t Act as iBeacons
author: David G. Young
---

Since we released the open source Android iBeacon Library a few months ago, allowing Android devices to detect iBeacon just like iOS, the stand-out feature that keeps Android devices from parity with iOS is the ability to *act* as an iBeacon.  The library is based on the Bluetooth LE support, added to Android 4.3 as part of its BlueDroid bluetooth stack.

Unfortunately, Bluetooth LE support in Android 4.3 and 4.4 only supports the “central” role, meaning it can act as a device that consumes Bluetooth LE data.  The “peripheral” role, which is used by devices being the Bluetooth LE data provider, is not supported.   This keeps Android devices from acting as an iBeacon, because iBeacons are nothing more than Bluetooth LE peripheral mode advertisers.  And if a device can’t advertise, it can’t act as an iBeacon.

But the BlueDroid stack isn’t the only game in town for Android Bluetooth LE.  Before Android 4.3 came out, Samsung created its own Samsung BLE SDK based on the open source Linux BlueZ bluetooth stack.  This bluetooth stack can be used to make iBeacons — our iBeacon Development Kit uses BlueZ command line utilities to make a Linux computer advertise as an iBeacon.  This left me wondering — is the same thing possible with the Samsung BLE SDK?

Their [Guides and Hints documentation](http://developer.samsung.com/ble)  (registration required) has conflicting things to say about this.  On Page 12 it says:

> The current version of the SDK supports only the GATT central role. Peripheral roles may be supported in future releases. 

This suggests that it has the same limitation of the core Bluetooth LE libraries in Android 4.3.  But later on it says:

> BluetoothGattServer: This class provides Bluetooth GATT server role functionality, allowing applications to create and advertise Bluetooth smart services and characteristics.

So I figured I’d write a little app to try this out. I got my hands on a Samsung Galaxy Tab 3 running Android 4.2. And followed the examples to write a little app to make it advertise.  The code looks like this:

```
public class MainActivity extends Activity {
	BluetoothProfile.ServiceListener mProfileServiceListener;

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		advertise();
	}
	
	public void advertise() {
		mProfileServiceListener = new BluetoothProfile.ServiceListener() {
			BluetoothGattServer mBluetoothGattServer = null;
			UUID mUuid = new UUID(0xdddd, 0xdddd);
			BluetoothGattServerCallback mServerCallback = new BluetoothGattServerCallback() {

				@Override
				public void onAppRegistered(int status) {
					if (status == BluetoothGatt.GATT_SUCCESS) {
						MutableBluetoothGattService bluetoothService = new MutableBluetoothGattService(mUuid,BluetoothGattService.SERVICE_TYPE_PRIMARY);
						MutableBluetoothGattCharacteristic characteristic = new MutableBluetoothGattCharacteristic(mUuid, status, status);
						bluetoothService.addCharacteristic(characteristic);
						// THE LINE BELOW STARTS ADVERTISING
						mBluetoothGattServer.addService(bluetoothService);
					}
				}
			};

			@Override
			public void onServiceConnected(int profile, BluetoothProfile proxy) {
				if (profile == BluetoothGattAdapter.GATT_SERVER) {
					mBluetoothGattServer = (BluetoothGattServer) proxy;
					mBluetoothGattServer.registerApp(mServerCallback);
				}
			}

			@Override
			public void onServiceDisconnected(int arg0) {
			}

		};

		BluetoothAdapter.getDefaultAdapter();
		BluetoothGattAdapter.getProfileProxy(this, mProfileServiceListener, BluetoothGattAdapter.GATT_SERVER);
	}
	
}
```

I hooked up a bluetooth sniffer then ran this code, verifying that the code to create and advertise the BLE service gets created.  But when I looked at the sniffer, I saw nothing.  The Samsung Galaxy Tab wasn’t transmitting anything.

After scratching my head a bit, I realized the fundamental problem was misleading documentation.  When Samsung says the BluetoothGattServer can advertise services, they don’t mean advertise as in the same sense as a Bluetooth peripheral device (like an iBeacon).  Because their SDK only supports the central role, “advertising” a service as a central server means sitting their quietly, only revealing (or “advertising”) its service characteristics to another device in peripheral mode after a connection is already established.  This connection establishment requires another device to do the actual radio advertising first.  Samsung’s SDK isn’t going to do it.

Unfortunately, this means that the Samsung BLE SDKs can’t be used to make Android devices transmit as iBeacons either.   What we need is an Android BLE API that allows creation of a peripheral server.  Lots of folks were hoping that peripheral role support would be added in 4.4, but it wasn’t.  Maybe we can hope for its addition in Android 4.5 or 5.0.  There is a [feature request](https://code.google.com/p/android/issues/detail?id=59693) asking for this.    Add your name to the list!

Don’t be too hard on Android, though.  Remember, OSX also only supported peripheral mode until Mavericks was released in October — and Apple has long been a far bigger booster of Bluetooth LE than Google.
