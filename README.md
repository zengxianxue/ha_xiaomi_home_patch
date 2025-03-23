# 米家HA集成状态同步补丁

此补丁是一个临时方案，用于解决米家HA集成中，设备状态不同步问题。通过定时器获取云端设备状态。

- 实现方法是在`miot/miot_client.py`中的`init_async`初始化时，添加一个定时器，每隔10秒获取一次云端设备状态。
- 此方法与执行`homeassistant.reload_config_entry`相比，设备状态不会先`unavailable`得到结果后再更新的情况(
  reload_config_entry会重新初始化集成)。

```python
async def __refresh_cloud_devices_timer(self) -> None:
    while True:
        _LOGGER.info('>>>>>>refresh_cloud_devices_timer<<<<<<, %s, %s', self._uid, self._cloud_server)
        await self.__refresh_cloud_devices_async()
        await asyncio.sleep(10)
```

**应用补丁**: `git apply refresh_cloud_devices_timer.patch`

