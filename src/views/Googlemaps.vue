<template>
    <div>
        <GoogleMap :api-key="apiKey" style="width: 100%; height: 500px" :center="{ lat: lat, lng: lng }" :zoom="19">
            <Marker :options="{ position: { lat: 52.039943, lng: 4.328807 }, icon: chargers[0]}" @click="changeIcon(1)">
                <InfoWindow>
                    <div id="contet">
                        <div id="siteNotice"></div>
                        <h1 id="firstHeading" class="firstHeading">Laadpaal 1</h1>
                        <div id="bodyContent">
                            <p>
                                Test
                            </p>
                        </div>
                    </div>
                </InfoWindow>
            </Marker>
            <Marker :options="{ position: { lat: 52.040052, lng: 4.328697 }, icon: chargers[1]}" @click="changeIcon(2)">
                <InfoWindow>
                    <div id="contet">
                        <div id="siteNotice"></div>
                        <h1 id="firstHeading" class="firstHeading">Laadpaal 1</h1>
                        <div id="bodyContent">
                            <p>
                                Test
                            </p>
                        </div>
                    </div>
                </InfoWindow>
            </Marker>
            <Marker :options="{ position: { lat: 52.040166, lng: 4.328587 }, icon: chargers[2]}" @click="changeIcon(3)">
                <InfoWindow>
                    <div id="contet">
                        <div id="siteNotice"></div>
                        <h1 id="firstHeading" class="firstHeading">Laadpaal 1</h1>
                        <div id="bodyContent">
                            <p>
                                Test
                            </p>
                        </div>
                    </div>
                </InfoWindow>
            </Marker>
            <Marker :options="{ position: { lat: 52.040268, lng: 4.328448 }, icon: chargers[3]}" @click="changeIcon(4)">
                <InfoWindow>
                    <div id="contet">
                        <div id="siteNotice"></div>
                        <h1 id="firstHeading" class="firstHeading">Laadpaal 1</h1>
                        <div id="bodyContent">
                            <p>
                                Test
                            </p>
                        </div>
                    </div>
                </InfoWindow>
            </Marker>
            <Marker :options="{ position: { lat: 52.040369, lng: 4.328613 }, icon: chargers[4]}" @click="changeIcon(5)">
                <InfoWindow>
                    <div id="contet">
                        <div id="siteNotice"></div>
                        <h1 id="firstHeading" class="firstHeading">Laadpaal 1</h1>
                        <div id="bodyContent">
                            <p>
                                Test
                            </p>
                        </div>
                    </div>
                </InfoWindow>
            </Marker>
            <Marker :options="{ position: { lat: 52.040443, lng: 4.328507 }, icon: chargers[5]}" @click="changeIcon(6)">
                <InfoWindow>
                    <div id="contet">
                        <div id="siteNotice"></div>
                        <h1 id="firstHeading" class="firstHeading">Laadpaal 1</h1>
                        <div id="bodyContent">
                            <p>
                                Test
                            </p>
                        </div>
                    </div>
                </InfoWindow>
            </Marker>
        </GoogleMap>
    </div>
</template>

<script>
import { GoogleMap, CustomMarker, InfoWindow, Marker } from 'vue3-google-map'
import { ref } from 'vue'
import { supabase } from '../supabase'

export default {
    name: "Googlemaps",
    components: {
        GoogleMap,
        CustomMarker,
        InfoWindow,
        Marker
    },
    setup() {
        // create data / variables
        const data = ref([]);
        const dataLoaded = ref(null);
        const chargers = [{icon_charger1: ""}, {icon_charger2: ""}, {icon_charger3: ""}, 
                        {icon_charger4: ""}, {icon_charger5: ""}, {icon_charger6: ""}];

        // get data
        const getData = async (chargerid) => {
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
        return (chargers)
        };

        // run data function
        for (let i = 0; i < chargers.length; i++) {
                getData(i);
            };
        console.log(chargers);
        console.log(Object.values(chargers[0]));

        return {chargers};
    },
    data() {
        return {
            apiKey: "AIzaSyAdKxFeQupIgn4N0mYqTyM1v5zRMXiw-mw",
            lat: 52.040150,
            lng: 4.328820,
        }
    },
    methods: {
        changeIcon(chargerid) {
            if (chargerid == 1) {
                if (this.icon1 == "https://i.ibb.co/xJxPbH8/normal-charger-red.png") {
                    this.icon1 = "https://i.ibb.co/ChDZHc1/normal-charger-green.png";
                }
                else {
                    this.icon1 = "https://i.ibb.co/xJxPbH8/normal-charger-red.png";
                }
            } else if (chargerid == 2) {
                if (this.icon2 == "https://i.ibb.co/xJxPbH8/normal-charger-red.png") {
                    this.icon2 = "https://i.ibb.co/ChDZHc1/normal-charger-green.png";
                }
                else {
                    this.icon2 = "https://i.ibb.co/xJxPbH8/normal-charger-red.png";
                }
            } else if (chargerid == 3) {
                if (this.icon3 == "https://i.ibb.co/xJxPbH8/normal-charger-red.png") {
                    this.icon3 = "https://i.ibb.co/ChDZHc1/normal-charger-green.png";
                }
                else {
                    this.icon3 = "https://i.ibb.co/xJxPbH8/normal-charger-red.png";
                }
            } else if (chargerid == 4) {
                if (this.icon4 == "https://i.ibb.co/xJxPbH8/normal-charger-red.png") {
                    this.icon4 = "https://i.ibb.co/ChDZHc1/normal-charger-green.png";
                }
                else {
                    this.icon4 = "https://i.ibb.co/xJxPbH8/normal-charger-red.png";
                }
            } else if (chargerid == 5) {
                if (this.icon5 == "https://i.ibb.co/y69FNM9/super-charger-red.png") {
                    this.icon5 = "https://i.ibb.co/Pgw3VKf/super-charger-green.png";
                }
                else {
                    this.icon5 = "https://i.ibb.co/y69FNM9/super-charger-red.png";
                }
            } else if (chargerid == 6) {
                if (this.icon6 == "https://i.ibb.co/y69FNM9/super-charger-red.png") {
                    this.icon6 = "https://i.ibb.co/Pgw3VKf/super-charger-green.png";
                }
                else {
                    this.icon6 = "https://i.ibb.co/y69FNM9/super-charger-red.png";
                }
            }
        }
    }
}
</script>