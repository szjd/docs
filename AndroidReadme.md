## App常期运行不退出
低内存时，不kill应用，最多只能写3个包名(注意格式，从包名尾部开始，每个包最多16个字符)
```
例如，
有以下这些包不希望低内存时被kill, com.tianqiao.luo.launcher3,com.cyj.wifigateway,com.cyj.voicerobotsbk
从包名尾部开始，每个包最多16个字符,在应用启动后写入。
    writeFile("/sys/xfocus/debug/lowmem_app", "luo.launcher3,voicerobotsbk,wifigateway");

    private void writeFile(String filePath, String data){
        try {
            File f = new File(filePath);
            FileOutputStream fos = new FileOutputStream(f);
            byte bytes[] = data.getBytes();
            fos.write(bytes);
            fos.close();
        } catch (IOException e){
            Log.d(TAG, e.toString());
        }
    }
```

## 获取设备名称
设备名称即是手机端连接设备时，呈现的名称，在背景音乐主机端可以使用如下属性获取
```
SystemProperties.get("persist.sys.jd.name") 
```
## 获取设备UUID
设备名称即是手机端连接设备时，呈现的名称，在背景音乐主机端可以使用如下属性获取
```
AccountManager accountManager = (AccountManager) getSystemService(ACCOUNT_SERVICE);
Account[] accounts = accountManager.getAccountsByType("com.judian.jdmediarender.account");
for(Account a : accounts){
    Log.d(TAG, "name=" + a.name + ",type=" + a.name);  
}
```
