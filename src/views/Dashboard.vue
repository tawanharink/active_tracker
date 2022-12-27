<template>
    <div
        class="grid grid-cols-1 sm:grid-cols-1 md:grid-cols-1 lg:grid-cols-1 gap-6"
    >
      <GoogleMap :api-key="apiKey" style="width: 100%; height: 500px" :center="{ lat: lat, lng: lng }" :zoom="19">
            <Marker :options="{ position: { lat: 52.039943, lng: 4.328807 }, icon: chargers[0]}" @click="updateCar(0)">
            </Marker>
            <Marker :options="{ position: { lat: 52.040052, lng: 4.328697 }, icon: chargers[1]}" @click="updateCar(1)">
            </Marker>
            <Marker :options="{ position: { lat: 52.040166, lng: 4.328587 }, icon: chargers[2]}" @click="updateCar(2)">
            </Marker>
            <Marker :options="{ position: { lat: 52.040268, lng: 4.328448 }, icon: chargers[3]}" @click="updateCar(3)">
            </Marker>
            <Marker :options="{ position: { lat: 52.040369, lng: 4.328613 }, icon: chargers[4]}" @click="updateCar(4)">
            </Marker>
            <Marker :options="{ position: { lat: 52.040443, lng: 4.328507 }, icon: chargers[5]}" @click="updateCar(5)">
            </Marker>
      </GoogleMap>
    </div>
</template>

<script>
import { ref } from "vue";
import { supabase } from "../supabase/init";
import { GoogleMap, Marker } from 'vue3-google-map'

export default {
  name: "dashboard",
  components: {
        GoogleMap,
        // CustomMarker,
        // InfoWindow,
        Marker
  },
  data() {
        return {
            apiKey: "AIzaSyAdKxFeQupIgn4N0mYqTyM1v5zRMXiw-mw",
            lat: 52.040150,
            lng: 4.328820,
        }
    },
    setup() {
        // create data / variables
        const data = ref([]);
        const dataLoaded = ref(null);
        const chargers = [{icon_charger1: ""}, {icon_charger2: ""}, {icon_charger3: ""}, 
                        {icon_charger4: ""}, {icon_charger5: ""}, {icon_charger6: ""}];
        //const carstatus = ref(null)

        // get data
        const getData = async () => {
            for (let chargerid = 0; chargerid < chargers.length; chargerid++) {
                try {
                    const { data: laadpalen, error } = await supabase.from('laadpalen').select('*').eq("id", chargerid);
                    if (error) throw error;
                    data.value = laadpalen[0];
                    dataLoaded.value = true;
                    //console.log(data.value);
                    //console.log(data.value["available"]);
                    if (chargerid < 4) {
                        if (data.value["available"]) {
                            chargers[chargerid] = "https://i.ibb.co/ChDZHc1/normal-charger-green.png";
                        } else {
                            chargers[chargerid] = "https://i.ibb.co/xJxPbH8/normal-charger-red.png";
                        }
                    } else {
                        if (data.value["available"]) {
                            chargers[chargerid] = "https://i.ibb.co/Pgw3VKf/super-charger-green.png";
                        } else {
                            chargers[chargerid] = "https://i.ibb.co/y69FNM9/super-charger-red.png";
                        }
                    }
                } catch (error) {
                    console.warn(error.message)
                }
            }
        console.log("data")
        return (chargers)
        };

        const updateCar = async (id) => {
        try {
            const { data, error } = await supabase
                .from('laadpalen')
                .select('id, available')
                .eq('id', id)
        if (error) throw error;
            if (data[0]["available"]) {
                try {
                    const { error } = await supabase
                        .from('laadpalen')
                        .update({ available: false })
                        .eq("id", id)
                        .select();
                    if (error) throw error;
                } catch (error) {
                console.log(error.message)
                }
            } else {
                try {
                    const { error } = await supabase
                        .from('laadpalen')
                        .update({ available: true })
                        .eq("id", id)
                        .select();
                    if (error) throw error;
                } catch (error) {
                    console.log(error.message)
                }
            } 
        } catch (error) {
            console.log(error.message)
        }
        getData();
    };

        // run data function
        getData();
        console.log(chargers);

        return {chargers, updateCar};
    },
};
</script>
