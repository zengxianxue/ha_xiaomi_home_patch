# 米家HA集成状态同步补丁

此补丁是一个临时方案，用于解决米家HA集成中，设备状态(在线/离线)不同步问题。通过定时器获取云端设备状态。

- 实现方法是在`miot/miot_client.py`中的`init_async`初始化时，添加一个定时器，每隔10秒获取一次云端设备状态。
- 此方法与执行`homeassistant.reload_config_entry`相比，设备状态不会先`unavailable`得到结果后再更新的情况(
  reload_config_entry会重新初始化集成)。
- 应用补丁后在HA中执行`homeassistant.reload_config_entry`会有`Session is closed`错误打印，但不影响效果。

**使用补丁**

按`ha_xiaomi_home`安装步骤，在`install.sh`前打补丁

```shell
cd config
git clone https://github.com/XiaoMi/ha_xiaomi_home.git
cd ha_xiaomi_home

# 下载补丁
wget https://github.com/zengxianxue/ha_xiaomi_home_patch/raw/refs/heads/main/refresh_cloud_devices_timer.patch
# 打补丁
git apply refresh_cloud_devices_timer.patch

./install.sh /config
```

