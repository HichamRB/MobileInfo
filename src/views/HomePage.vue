<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Device Info</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content>
      <p>ID: {{ deviceId }}</p>
      <pre>{{ deviceInfo }}</pre>
      <pre>{{ batteryInfo }}</pre>
      <p>Language Code: {{ languageCode }}</p>
    </ion-content>
  </ion-page>
</template>
<script lang="ts">
import { defineComponent, ref, onMounted } from 'vue';
import { Device } from '@capacitor/device';

interface DeviceId {
  uuid: string;
  // Add other properties as needed based on what Device.getId() returns
}

export default defineComponent({
  name: 'DeviceInfoPage',
  setup() {
    const deviceId = ref<DeviceId>({
      uuid: '',
    });

    const deviceInfo = ref<any>(null); // Adjust type based on actual data structure
    const batteryInfo = ref<any>(null); // Adjust type based on actual data structure
    const languageCode = ref<string>('');

    const fetchDeviceId = async () => {
      try {
        const id = await Device.getId();
        deviceId.value = id as DeviceId;
      } catch (error) {
        console.error('Error fetching device ID:', error);
      }
    };

    const fetchDeviceInfo = async () => {
      try {
        const info = await Device.getInfo();
        deviceInfo.value = info;
      } catch (error) {
        console.error('Error fetching device info:', error);
      }
    };

    const fetchBatteryInfo = async () => {
      try {
        const info = await Device.getBatteryInfo();
        batteryInfo.value = info;
      } catch (error) {
        console.error('Error fetching battery info:', error);
      }
    };

    const fetchLanguageCode = async () => {
      try {
        const code = await Device.getLanguageCode();
        languageCode.value = code.value;
      } catch (error) {
        console.error('Error fetching language code:', error);
      }
    };

    onMounted(async () => {
      await fetchDeviceId();
      await fetchDeviceInfo();
      await fetchBatteryInfo();
      await fetchLanguageCode();
    });

    return {
      deviceId,
      deviceInfo,
      batteryInfo,
      languageCode,
    };
  },
});
</script>




<style scoped>
ion-content {
  --padding: 16px;
}
</style>
