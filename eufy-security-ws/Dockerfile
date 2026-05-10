ARG BUILD_FROM
FROM $BUILD_FROM
ARG EUFY_SECURITY_WS_VERSION
WORKDIR /usr/src/app
RUN \
    set -x \
    && apk add --no-cache \
    nodejs \
    npm \
    jq \
    && npm install --force \
    "eufy-security-ws@${EUFY_SECURITY_WS_VERSION}" \
    && sed -i 's/return types_1.DeviceType.LOCK_8530 == type;/return types_1.DeviceType.LOCK_8530 == type || 189 == type;/' /usr/src/app/node_modules/eufy-security-client/build/http/device.js \
    && sed -i 's/else if (this.isLockWifiT8520P()) {/else if (this.isLockWifiVideo()) { metadata = { ...types_1.DeviceProperties[types_1.DeviceType.LOCK_8530] }; } else if (this.isLockWifiT8520P()) {/' /usr/src/app/node_modules/eufy-security-client/build/http/device.js \
    && sed -i 's/isLockWifiT8506() {/isLockWifiT85D0() { return Device.isLockWifiT85D0(this.rawDevice.device_type); } isLockWifiT8506() {/' /usr/src/app/node_modules/eufy-security-client/build/http/device.js \
    && sed -i 's/if (device_1.Device.isLock(stationData.device_type) \&\&/if ((device_1.Device.isLock(stationData.device_type) || device_1.Device.isLockWifiVideo(stationData.device_type)) \&\&/' /usr/src/app/node_modules/eufy-security-client/build/http/station.js \
    && sed -i 's/else if (device.isLockWifiVideo()) {/else if (device.isLockWifiVideo() \&\& false) {/' /usr/src/app/node_modules/eufy-security-client/build/http/station.js \
    && sed -i 's/device.isLockWifiT8506() ||/device.isLockWifiVideo() || device.isLockWifiT85D0() || device.isLockWifiT8506() ||/' /usr/src/app/node_modules/eufy-security-client/build/http/station.js \
    && sed -i 's/\[PropertyName.DeviceLockStatus\]: exports.DeviceBasicLockStatusProperty,/[PropertyName.DeviceLockStatus]: exports.DeviceAdvancedLockStatusProperty,/' /usr/src/app/node_modules/eufy-security-client/build/http/types.js
COPY run.sh /
RUN chmod a+x /run.sh
WORKDIR /data
CMD [ "/run.sh" ]
