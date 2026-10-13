# DingDingDK
#include <WiFi.h>

// =====================【在这里修改成你公司打卡WiFi信息】=====================
const char* targetSSID = "CompanyWiFi";    // 公司打卡WiFi名称
uint8_t targetBSSID[] = {0xAA,0xBB,0xCC,0xDD,0xEE,0xFF}; // 公司路由器BSSID(MAC地址)
int wifiChannel = 6;  // 公司WiFi信道（1/6/11常用，务必填对）
// =========================================================================

void setup() {
  Serial.begin(115200);
  delay(500);
  Serial.println("ESP32C3 钉钉WiFi打卡AP启动");

  // 设置软AP的BSSID（伪造路由器MAC）
  WiFi.softAPsetHostname(targetSSID);
  WiFi.softAPConfig(IPAddress(192,168,4,1), IPAddress(192,168,4,1), IPAddress(255,255,255,0));

  // 启动软AP，无密码，信道和公司WiFi一致
  bool apStart = WiFi.softAP(targetSSID, NULL, wifiChannel, 0, 1);
  if(apStart){
    Serial.println("✅ AP创建成功");
  }else{
    Serial.println("❌ AP启动失败");
  }

  // 修改AP的MAC(BSSID)
  esp_wifi_set_mac(WIFI_IF_AP, targetBSSID);

  Serial.print("AP SSID:");Serial.println(targetSSID);
  Serial.print("AP BSSID:");
  for(int i=0;i<6;i++){
    Serial.printf("%02X", targetBSSID[i]);
    if(i<5)Serial.print(":");
  }
  Serial.println();
}

void loop() {
  delay(1000);
}
