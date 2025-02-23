---
id: 46f1m4lbo2q17a8q49bxsvx
title: Permission
desc: ''
updated: 1713020582116
created: 1713020356636
---

#### 无法访问文件
```c++
read file fail/sdcard/Android/data/xjf2613/test_1920x1080_nv21
01:24:09.505 20897-20949 System.err         com.example.nativeprj1       W  java.io.FileNotFoundException: /sdcard/Android/data/xjf2613/test_1920x1080_nv21
01:24:09.505 20897-20949 System.err         com.example.nativeprj1       W  	at android.content.res.AssetManager.nativeOpenAsset(Native Method)
01:24:09.505 20897-20949 System.err         com.example.nativeprj1       W  	at android.content.res.AssetManager.open(AssetManager.java:833)
01:24:09.505 20897-20949 System.err         com.example.nativeprj1       W  	at android.content.res.AssetManager.open(AssetManager.java:810)
01:24:09.505 20897-20949 System.err         com.example.nativeprj1       W  	at com.example.nativeprj1.MainActivity.readNV12File(MainActivity.java:96)
01:24:09.505 20897-20949 System.err         com.example.nativeprj1       W  	at com.example.nativeprj1.MainActivity$1MyThread.run(MainActivity.java:53)

public static byte[] readNV12File(Context context, String fileName) {
        AssetManager assetManager = context.getAssets();
        byte[] nv12Data = null;
        try {
            // 打开文件输入流
            InputStream inputStream = assetManager.open(fileName);
            // 读取文件数据
            nv12Data = new byte[inputStream.available()];
            inputStream.read(nv12Data);
            // 关闭输入流
            inputStream.close();
        } catch (IOException e) {
            Log.e("TAG", "read file fail" + e.getMessage());
            e.printStackTrace();
        }
        return nv12Data;
    }

    无法读取

```


#### 无法进行文件拷贝
```c++
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />

    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.MANAGE_EXTERNAL_STORAGE" />
    <application
        android:requestLegacyExternalStorage="true"
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.NativePrj1"
        tools:targetApi="31">
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>


</manifest>

String folder = getFilesDir().getPath();
//        String folder = "/sdcard/Android/data/xjf2613";
public static void copyRawResourceToFile(Context context, int resourceId, String filePath) {
        InputStream inputStream = context.getResources().openRawResource(resourceId);
        OutputStream outputStream = null;

        try {
            outputStream = new FileOutputStream(filePath);
            byte[] buffer = new byte[1024];
            int length;
            while ((length = inputStream.read(buffer)) > 0) {
                outputStream.write(buffer, 0, length);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (inputStream != null) {
                    inputStream.close();
                }
                if (outputStream != null) {
                    outputStream.close();
                }
            } catch (IOException e) {
                Log.e("TAG", "copyRawResourceToFile fail" + e.getMessage());
                e.printStackTrace();
            }
        }
    }
解决``
```