<script setup>
import { ref, onMounted } from "vue";
import { getDataFromDatabase, onDatabaseDataChange } from "../firebase";

const databaseData = ref({});
let mapContainer = ref(null);
const map = ref(null);
const userMarker = ref(null);
const deviceMarker = ref(null);

const loadGoogleMaps = () => {
  return new Promise((resolve, reject) => {
    if (window.google && window.google.maps) {
      resolve();
      return;
    }

    const script = document.createElement("script");
    script.src = `https://maps.googleapis.com/maps/api/js?key=AIzaSyCKVvI-0R1P3Bd-5L8JH--PRSTSEDWwHo0&callback=initMap`;
    script.async = true;
    script.defer = true;
    script.onerror = reject;
    window.initMap = () => resolve();

    document.head.appendChild(script);
  });
};

onMounted(async () => {
  await loadGoogleMaps();

  map.value = new google.maps.Map(mapContainer.value, {
    center: { lat: 0, lng: 0 },
    zoom: 14,
  });

  const userIcon = {
    url: "http://maps.google.com/mapfiles/ms/icons/red-dot.png",
    scaledSize: new google.maps.Size(40, 40),
  };

  const deviceIcon = {
    url: "http://maps.google.com/mapfiles/ms/icons/blue-dot.png",
    scaledSize: new google.maps.Size(40, 40),
  };

  userMarker.value = new google.maps.Marker({
    map: map.value,
    title: "Your Location",
    icon: userIcon,
  });

  deviceMarker.value = new google.maps.Marker({
    map: map.value,
    title: "Device Location",
    icon: deviceIcon,
  });

  const userInfoWindow = new google.maps.InfoWindow({
    content: "<b>You are here</b>",
  });

  const deviceInfoWindow = new google.maps.InfoWindow({
    content: "<b>Device Location</b>",
  });

  userMarker.value.addListener("click", () => {
    userInfoWindow.open(map.value, userMarker.value);
  });

  deviceMarker.value.addListener("click", () => {
    deviceInfoWindow.open(map.value, deviceMarker.value);
  });

  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition((position) => {
      updateUserLocation(position.coords.latitude, position.coords.longitude);
    });
  }

  try {
    const paths = ["locationCoordinate/latitude", "locationCoordinate/longitude"];

    for (const path of paths) {
      const data = await getDataFromDatabase(path);
      databaseData.value[path] = data;

      onDatabaseDataChange(path, (newData) => {
        databaseData.value[path] = newData;
        updateFirebaseLocation();
      });
    }
  } catch (error) {
    console.error("Error fetching data from Firebase:", error);
  }
});

const updateUserLocation = (lat, lng) => {
  map.value.setCenter({ lat, lng });
  userMarker.value.setPosition({ lat, lng });
};

const updateFirebaseLocation = () => {
  const firebaseLat = parseFloat(databaseData.value["locationCoordinate/latitude"]);
  const firebaseLng = parseFloat(databaseData.value["locationCoordinate/longitude"]);

  if (!isNaN(firebaseLat) && !isNaN(firebaseLng)) {
    deviceMarker.value.setPosition({ lat: firebaseLat, lng: firebaseLng });
  }
};
</script>

<template>
  <div ref="mapContainer" style="width: 100%; height: 400px"></div>
</template>
